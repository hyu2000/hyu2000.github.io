It was said that professional Go players spend most of their time solving puzzles, typically life and death
problems (LnD).
AlphaZero never did any puzzles. Its learning is completely unscripted. Can it solve puzzles correctly?
Can we speed up learning by adding puzzle solving to its curriculum?

The answer to the first question is likely yes. Of course AZ knows nothing about what a puzzle is really
about. We could fill up the rest of the board so that the outcome of a corner will decide winner of the
entire board. A well-trained bot is able to solve all puzzles I have tried. But it is a tedious job to
fill up a board properly, so I didn't test many.

Can we train a bot that can solve puzzles? This turns out to be a great way in understanding 
how AZ training makes progress. 

# insights from working on a simple puzzle
Consider this simple puzzle https://online-go.com/puzzle/15438: black to kill. Even a beginner would
know that H1 is the key, the only place where two eyes could be made. But to a randomly-initialised bot,
the entire board is eligible; it hasn't learned how capture works either.

It learned the solution amazingly fast. After just 24 attempts, it solves the puzzle correctly (and consistently).
[Problem 1-5-80975966024.sgf](view-games.html)

It bumped into the key move by chance. In [Problem 1-3-80186480014.sgf](view-games.html), it found the the key move (H1), but
didn't actually know that it can kill off the white corner. In the next iteration, [Problem 1-4-80764325131.sgf](view-games.html),
it tried the key move again. Importantly, it somehow followed up with J1 (could be partially luck as well). 
After white G1 capture, it seemed to know (and carried out) the simpler kill (that every beginner knows). 

This seems to be a key moment in the learning process. It has found a completely different outcome: black
wins rather than white wins. After this bot consistently goes for H1 first, and killed white in every try.

How do we explain this switch? Model update is normally a gradual process. Here we observed a sudden switch
of behavior, as if the model had an "aha" moment. I believe that policy change is indeed gradual, but 
the value update for the H1-J1 state is crucial, which probably flipped from negative to positive. Coupled
with MCTS, black is going to like this branch a lot, since it leads to the most promising outcome.

Now, should we declare bot has solved this puzzle? Not quite yet. If we look at the details, while black
chooses H1 mostly, the Q-value is still slightly negative, i.e. it still thinks white will win the battle,
however slightly. After the next G1 move, it starts to believe it will win a little more. This is analogous
to a player who has just discovered a new trick, and is gingerly experimenting with it more, with confidence
growing if the results hold.

# any hope of definite answer?
For more advanced puzzles, [example](https://online-go.com/puzzle/42943), I have seen the bot bumped into the 
solution move (for the very first move), 
after a while switching to another opening, and later switch back to the correct opening, not unlike a player
wavering between different beliefs. The criteria of a bot mastering an LnD puzzle is to know the outcome
from the very beginning. But even then we are not completely sure. Maybe there is a trap state that makes
the correct outcome hard to discover? Maybe it just bumped into *a* solution, and could be surprised into
a wrong response if opponent picks a different move?

AlphaZero didn't attempt to answer any of those questions: even 9x9 Go is complicated enough to know anything
for certain. However, with puzzles, simple ones at least, the answers seem attainable.