
# AlphaGo lineage
Well, it's been a fun journey. DeepMind has made a movie about AlphaGo. Here I mostly try to trace back its
academic lineage.

MCTS, RLGo, DQN (Atari, 2013), DNN for policy, AlphaGo (2016), AlphaGoZero, AlphaZero, Capture The Flag, AlphaStar (2019).

At this point, AlphaGo is super-human. In terms of machine "learning", we have little to teach it anymore. 
However, interesting things can be said about open-ended learning. Deepmind has published a few papers:
- Open-ended learning in Symmetric Zero-sum games (2019)
- Real World Games Look like Spinning Tops

They came from a game-theory point of view, and put AGZ-style training in a firmer theoretic footing.
For example, some (SAI) had observed self-play may not always lead to monotonic increase in performance.
This is explained by the "spinning tops"-shaped gamescape quite well.
