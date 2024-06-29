One of the main challenges in AZ experiments is computing power. When reading the first AlphaGo
paper, I'm aware of the thousands of machines they deployed for self-play. But for 9x9 board, the complexity
should be a lot lower. As a matter of fact, the SAI paper said 9x9 reduces the amount of computation by 9 orders of magnitude: games are shorter (roughly 9x9 rather than 19x19); input feature maps (to DNN) smaller; branching factor
in the search tree smaller. All in all, I figured that I can probably get away with a shoe-string compute
budget: my M1 macbook pro, which has just come out, with a relatively decent GPU. In the worst case, I will let 
it run for a couple weeks, or a month. After all, I have only a modest goal and there are no deadlines to hit.

When I started out, I decided to use Python. First of all, it's a hobby project. Plus, Python is the go-to choice 
for deep learning: Tensorflow, PyTorch, etc. Most of the heavy-lifting would be handled by Tensorflow, 
which is in C++ and heavily optimized.

## MCTS
For every move in a game, AZ needs to perform hundreds of simulations to look into the future. This is the 
improvement operator, the main workhorse behind AZ's success. A naive implementation in Python would be very slow.
I used the minigo Python codebase, whose MCTS implementation is discussed in Brian Lee's 
[excellent article(https://www.moderndescartes.com/essays/deep_dive_mcts/)]. The key idea is to use NumPy to vectorize
as much as possible, and use virtual-losses to batch up NN evaluations.

## Parallel workers and difficulty in caching
Every programmer would turn to parallelization and caching to speed up slow code. Running multiple workers is 
straightforward, except that sometimes a worker would hang for no obvious reason. It's not as easy to cache 
expensive NN evaluations across multiple workers in Python, given that I'm using multiprocessing. That is, 
the cache has to work across process boundary. Multi-threading would be much more preferable. Alas, I am using
Python.

With parallel workers, I can run 2000~3000 self-play games on my laptop in 5~6 hours. It is painfully slow, and the 
occasional hang is a nuisance (deep inside some Tensorflow ops which I never figured out). 
But I was able to see progress in game quality, after several iterations.

After quite some research on what's behind the TF hang on tensorflow-metal, not understanding why Apple
would not devote more resource into this project, I stumbled upon CoreML library that Apple actually put
its weight behind. It can convert TF models into its own format, and run inference in parallel with no 
issues. To put icing on the cake, it is much more efficient: self-play time is down by almost half!

Tensorflow2 provides tf.function that can compile the full compute graph, which gives sizeable speedup.
More on this later.

## Profiling result, game logic
After performance improvements mentioned above, I ran cProfile on several selfplay games, to understand where 
the bottleneck is. 4 selfplay games took a total of 45 seconds, of which:
- 13 seconds for DNN evaluation
- 13 seconds in pure game logic! MiniGo code tracks liberties of stones for every move, which takes time to update.
  Also, figuring out what moves are legal is expensive too. Keep in mind that we need to perform these operations
  for every MCTS expansion (400 per move).
- the rest are scattered around, no clear culprit to optimize on.

To be fair, MiniGo codebase tried hard to use NumPy whenever possible, including for game logic, so it's 
not a poor point to start from. Nevertheless, one has to accept the tradeoff from using Python. Would Cython
help? More on this later.

## Speedups: symmetry, Zobrist's hash, Benson's scoring
I also tried some Go-specific techniques. The Go board is symmetric, in 8 ways of various rotations and flips.
Unfortunately there is no simple way to tell a DNN that all eight boards are really the same. But we can
exploit this in game logic. We only need to explore one variant in self-play, and later enhance NN training 
data using symmetries. How much does this save? In a 9x9 game, the reasonable opening move is in the center
5x5 square. Using symmetry, we only need to explore a quadrant of size 6.

Another way to speed up a game is to shorten it: why running full MCTS if a game is almost over. 
Humans seldom finish a game. We just know it's over: isolated stones have no chance of surviving. 
But this is quite hard for a machine to know. Tromp-Taylor scoring basically treats all stones on the board as 
alive. Benson's algorithm can decide the outcome of a game faster than Tromp-Taylor scoring can, and is more accurate.

## JAX
JAX is the new kid in town, but it's very impressive when dealing with heavy computation. Through
JIT, Python code can run as fast as native code. With device-specific kernel optimization, we could be looking at
orders of magnitudes in speedup. In a sense, it probably grew out of tf.function.
