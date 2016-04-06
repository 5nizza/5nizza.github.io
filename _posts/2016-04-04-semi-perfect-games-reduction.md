---
title: Note on Semiperfect-information Games
layout: post
---

I am interested in sure-winning objectives (no probabilities), so we will look at
[Semiperfect-information Games](https://chess.eecs.berkeley.edu/pubs/78.html)
by Chatterjee and Henzinger from this prospective.
Note that this prospective of view make the paper somewhat redundant
(and it is probabilities that make the result interesting).

Here we will look into reduction of semiperfect into perfect information games.
This reduction does not incur an exp blow-up as
in the general case of imperfect information games.

## Semiperfect games
What are semiperfect games?
The formal definition from the paper is a bit complicated,
although the idea is simple.

1. Imagine our game graph consists of environment and systems states,
   partitioned into (non-intersecting) observations.
   The system knows only observations,
   but the environment knows the exact states.
   This asymmetry alone does not give any benefits ([games-tutorial,p10][games-tutorial]),
   i.e.,
   the system wins in symmetrically defined game
   iff
   it wins in the game where the environment knows the exact state [^fn1].

2. Now imagine that env states have no partitions (i.e., singleton partitions).

3. Finally, additionally restrict transitions from the environment states:
   if from env state $e$ there is a transition into some system state of observation $O$,
   then every system state $s$ of $O$ has the transition $e \rightarrow s$.
   Thus, the game graph is of the form:
   <img src="{{ site.url }}/assets/semiperfect-game-graph.jpg" width="100%"/>

<br/>

## Reduction
These restrictions make it very simple to solve such games.
Originally, the paper goes via reduction to perfect information _concurrent_ games,
but consider only sure-winning,
then the following reduction to perfect information turn-based ("standard") games
seems to make the trick:

<img src="{{ site.url }}/assets/semiperfect-game-reduction.jpg" width="100%"/>

Where on the left side we have the piece of a semiperfect game,
and on the right -- the perfect information variant
(first, the system moves, then the env resolves the non-determinism).

Note that this reduction is very similar to the original reduction from the paper:

<img src="{{ site.url }}/assets/semiperfect-game-original-reduction.jpg" width="50%"/>

where we use numbers to distinguish moves made by the environment
(thus $(a,2)$ means env and sys simultaneously move $2$ and $a$ resp.).

#####Footnotes

[^fn1]: "The system wins in symmetrically defined game iff it wins in the asymmetric version",
      because, intuitively,
	  for the system to win it needs to win agains _any_ environment strategy.



[games-tutorial]: http://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.183.2113
