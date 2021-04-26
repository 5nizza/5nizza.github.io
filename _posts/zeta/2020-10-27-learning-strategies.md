---
title: "Applications of Automata Learning in Verification and Synthesis"
tags: [learning, 'safety games']
layout: post
---

[paper](https://core.ac.uk/download/pdf/36614106.pdf)\
[bibtex]({{ site.url }}/assets/bibs/neider14.bib)

(This is a whole thesis, but now I focus on Chapter 7.)

The focus is on synthesising _small_ strategies for safety games.
The standard approach is to solve the game using the fix-point computation,
get a nondeterministic strategy (also called maximally permissive),
then determinise it.
Thus, this approach uses the states of the game arena as its memory,
while the strategy itself is positional aka memoriless.
This approach provides no guarantees on the size of the resulting strategy,
where the size is in the terms of arena states that it can visit.

In the thesis, they suggest instead to _learn_ a Mealy machine representing
a winning strategy of the game.
Since active learning algorithms, like Angluin's L\*, typically learn automata
in increasing-size order, they allow for finding small implementations first.

However, to make this idea work, they needed to adapt L\* algorithm.
The original learning algorithm, on discovering a new state,
explores all possible ways to play from that state.
This means that L\* explores all possible output actions,
while we are interested only in _one_ possible output action.
They adopted the L\* algorithm, as well as Kearns and Vazirani’s algorithm,
to account for that.

_High-level idea_.
A maximally permissive strategy can be viewed as an automaton,
with its language being a subset of the input-output action words.
The learner then wants to learn a strategy automaton
that is a subset of this MPS automaton
and whose input-actions domain is total.
Thus,
in the learning algorithm
the equivalence query is replaced by checking whether the candidate strategy is winning.
Their algorithm proceeds as follows:

1. Computes a winning region and then the MPS automaton.
2. Learn a DFA that is a subset of the MPS and that has a total input domain.

They mention that the quality of the learned DFA depends on the quality of CEXs
that the teacher provides.
They use the shortest CEXs.

_Modification of the L\* algorithm_.
Recall that the original L\* algorithm, on discovering a new state,
explores all possible output actions.
Therefore, without modifications,
the learned strategy will be the whole MPS,
while we care only about its deterministic subset.
To avoid this,
they maintain a set of possible output actions, initially set to $\varnothing$;
note that having just one such output action is sufficient.
The original L\* algorithm maintains a table that is consistent and closed.
In their variant, the algorithm forms a candidate solution as soon as the table is consistent
while dropping the closedness constraint.
[The details I have not yet understood.]
They also develop a modification of another learning algorithm that seems to be better.

