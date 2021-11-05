# Dispatch from the trenches: from 5x5 Go to 9x9 Go
While 5x5 Go is fun, it feels like a puzzle more than a typical Go game. No corner fight -- the entire board is about 
the size of a corner; no balance between territory and influence -- the board is just so small; no strategies.

I am therefore looking forward to training on 9x9 board. The concepts of 19x19 board is relevant here. 
How would the bot learn life and death? How would it know when to invade, and when not to?

### Alas, experiments took much longer to run on 9x9 board
For the 5x5 Go experiments, I was able to complete 5~6 iterations of selfplay overnight (on a single machine, since
it's just me). But now I can finish only
one 9x9 Go iteration overnight. Most of the papers indicated that I'd need at least 100 iterations to get to a good
strength. And that's assuming I've got all hyper-parameters set properly.

### Would I learn some insights?
In any case, it's the same'o AlphaZero selfplay code. But instead of running 10+ iterations, I may have to run 200 iterations
before getting something decent. From the literature, somehow AZ just learns: from being a monkey putting stones 
randomly on the board, to a human beginner, happy-go-merry with capturing stones, to showing off some interesting corner
patterns, to becoming a decent player, and finally to super-human.

But since my experiments took overnight to complete just one iteration, I got plenty time to mull over whether it is
progressing as expected. Maybe I didn't have the hyper-parameters right? Maybe it's just the natural up-and-downs, and I was 
just being paranoid? If I had more cpu/gpus, I'd have blown through a bunch of experiments already.

### How AZ learns
This paper is lesser known, but the author claimed to have discovered AZ algorithm independently. And he
explains the motivation behind AZ really well: 
"Thinking fast and slow with Deep Learning and Tree Search", by Thomas Anthony et al. 

The two main components of AZ, neural network and tree search, combine to work in a way like a real human player.
When facing a particular board situation, we have some intuitions from just looking at the board (deep neural network).
This is the "thinking fast" part.
Then we plan out several steps into the future to see where it leads to, just like MCTS would. Good players can plan
out more steps (more MCTS simulations), or they have better intuition (better DNNs), or both. 

There are several hypothesis on how AZ learns the game. Naturally, one would have to learn the end-games before it
can tackle openings or middle-game. (Any Go player can tell you how important it is to master the corner life-and-death
patterns. If you are the defender of a corner, unless you are sure that it cannot be successfully invaded, you either have
to beef up the defense, or keep in mind how much an invasion can cost you.)

But in AZ, since everything is embedded in a single deep neural network, and it is trying only to maximize 
win-rate, nothing else, it is hard to delineate exactly what it's learning. It could be learning
everything at the same time. ELF OpenGo has designed some experiments to answer this question, but the results
are not completely clear.