Each AlphaGo model, if you think about it, actually contains two sub-models: black and white. In AGZ training,
they compete with each other and are updated simultaneously to produce more competitive games. It is so natural
to view them as a single model -- human players don't distinguish the two models in our mind. On the other hand, you could also
see AGZ in the light of GAN, where the dynamics between generator and discriminator can get a bit tricky to deal
with. If everything works out, GAN produces beautiful images that's as realistic as the original. Same for AGZ:
bots evolves to a style that flows almost effortlessly. But we need to understand that "if" part better.

# two models in one: how to derive one from the other
Are you good at playing black, but not white, in a board game? Funny question. Possible, but not likely (that is, if you 
are willing to try just a bit harder).

We can easily derive a white model from a black model. To evaluate a white position, we simply search one step
forward to evaluate the subsequent black states with the black model. Such derived white model is compatible with
the black model. Instead of separating black and white as two neural nets, we benefit from parameter sharing
by combining both into a single neural net. This is the same motivation behind AGZ's dual-net structure where
value and policy network shares the same trunk.

# many facets of an AGZ model
We could go a bit further: are you good at playing the opening of a game, the middle, or the endgame? If the question
in the previous section (whether you are good at playing black vs white) sounds a bit queer, we can surely 
relate to this one. An AGZ model has to do all of the above. 
If we have a bunch of AGZ models, and have somehow determined that one is good at opening, one at middle, one for endgame, we could combine all
three to build a super-model. Well, the fact is that a model good at opening must be good at endgame, because the accuracy of 
the valuation of an early board hinges on that of later board states. 

In fact, if you watch AGZ training closely, you can see the interplay between the white model and black model: black
may have some favorite opening moves at one point and is winning handsomely, then completely switch off to a different 
opening in the next generation. This is all because white doesn't want to be in the same losing situation anymore.
It has decided to try something different. Black, as a result, needs to adjust too.

# convergence in an ever-changing environment
Michael Littman wrote a very interesting paper back in 1994 
"Markov games as a framework for multi-agent reinforcement learning", which shed 
light on a fundamental AGZ design: the two-agent issue we are discussing here. If you have studied classic reinforcement
learning, you would know Markov Decision Process, Q-learning and such, where the setup is always an agent trying to
maximize rewards in some environment. The environment is generally stochastic, but always stationary. In Go (and other
board games), we need to ask the question: what is the environment? For the black player, white should be part of the
environment; and vice versa. This is fine, until we realize that in AGZ, both are adapting at the same time. In other words,
the environment for black is changing all the time -- we are dealing with adaptive agents.

In this light, it's quite amazing that AGZ training actually converges. The resurgence of deep neural net
is largely due to numerous efforts to make sure gradient descent will converge at a reasonable pace. Now we have made
the environment non-stationary. I must admit that many times in AGZ training, I have no faith that it would lead to
better models. But it has largely panned out in the end.

# minimax extension of Q-learning: learning a conservative strategy
In the Littman paper, he extended Q-learning to minimax-Q learning, for two-player zero-sum games. The new
algorithm will learn the minimax strategy. His setup is a bit different from Go: instead of turn-taking, two players
take simultaneous action, as in rock-paper-scissor. To avoid being second-guessed, the best strategy for each player is
to be conservative: mini-max, where one tries his best assuming the opponent will adopt the strongest counter-measure.

With a wave of hand, I claim that AGZ training leads to minimax policy as well. Minimax policy is conservative in the
sense that it is targeting any kind of opponent, including the strongest opponent. For AGZ, the strongest opponent is
itself.
