
Well, it's been a fun journey. DeepMind has made a movie about AlphaGo. Here I mostly try to trace back its
academic lineage.

# AlphaGo lineage

MCTS, RLGo, DQN (Atari, 2013), DNN for policy, AlphaGo (2016), AlphaGoZero, AlphaZero, MuZero, Capture The Flag, AlphaStar (2019).

# Meta-Game perspective, gamescape, open-ended learning
Most people approach Go from the perspective of finding better/optimal moves (policies). But as we run self-play,
 we get a series of agents, each typically stronger than the previous one. But you would be mistaken to think
that Elo is monotonically increasing. While the trend might be increasing in general, at some point you would
run into some doubts, and have to think harder.

As a matter of fact, that's what DeepMind realized (even as early as the original AlphaGo work). A bunch of work eventually
led to population-based training and AlphaStar (an elaborate league-based training), along with a number of interesting papers 
leans heavily on game theory.

Meta-game analysis basically examines payoff table between a bunch of agents. This is at such a high level that it
doesn't even bother to look at individual moves. Instead of playing a game, you are the manager of a number of
agents (players). You are playing with strategies. The task is to understand your players and win games by choosing
your players wisely. Self-play produced a series of players. (Un)Fortunately, they are more than one-dimensional.
The strategy space is rich, and we would need to expand the diversity of the population (gamescape), so as not to
be blinded by a novel style of play.

These papers are actually fun to read:
- Re-evaluating Evaluation, 2018
- Open-ended learning in Symmetric Zero-sum games (2019)
- Real World Games Look like Spinning Tops, 2020
- AlphaRank: Multi-Agent Evaluation by Evolution, 2019
