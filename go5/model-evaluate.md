AlphaZero showed the way to train super-human Go bots. You can play them online, likely losing all the
time. That's not fun. They also offer move-by-move analysis, showing where the best move is, and how much
you lose by playing the actual move. If you are diligent, you can learn quite a bit and improve your 
Go skills.

The other day I decided to play a smaller model that is probably at the professional level, not the best.
I still lost, but game felt much closer and more exciting. After the game, I looked at the analysis, but
can't help wondering whether the advices given are completely valid. What if I used a stronger bot? Surely
the advice would be different. In AlphaGo training, we get a trail of models, hundreds of them, with increasing 
strength, from random play, to novice level, then amateur, professional, and finally super-human. Each one of them
can play and provide analysis. Facing the same board, each may prefer a different move, together with a detailed 
analysis why that is the best. This can be quite confusing. As the saying goes, with many clocks, you
can't tell the time anymore.

Come to think of it, this is just like playing Go in a street corner with a bunch of spectators, each offering
"expert" commentaries. How do we handle this in the real world? Well, there are a couple choices. One can take
the consensus, the popular vote. But without knowing the crowd, we are risking that the expert 
opinion might be in minority and therefore drowned out. A better way is to rank the crowd by their skill levels,
and take the best opinion. This takes us to the exciting world of ranking. Professional players participate
in tournaments. The outcome of those games are analyzed by a ranking system which decides on their dan-level. 
The system exists in other sports too, for example, tennis. For bots, Elo ranking is commonly used. Roughly
speaking, Elo measures the odds of winning between players.

While it is nice to have a single number for ranking, this can be over-simplifying. Go, as a complex game,
involves many skills: opening, joseki, close-fighting, corner patterns, end-games. Two players may have 
similar Elo ratings, but with different strengths and weaknesses in each area. 

Now, back to my original situation, two bots offer different analysis of the same board, what do we do?
Suppose we know their Elo rankings now. We should probably go with the stronger bot, but no guarantee that 
is the right choice. Even with the same model, we can change
the amount of searches and get a different move. The key here, I think, is to adopt a *probabilistic*
way of thinking. This is the paradox of board games. They are clearly deterministic with perfect information.
We know there is a minimax solution. But we don't know it. Out of the shear search space rises uncertainty.
60% chance of winning. Deal with it.
