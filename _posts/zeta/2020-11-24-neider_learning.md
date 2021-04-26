---
title: Neider's work on learning
layout: post
tags: [learning]
---

Daniel Neider has lots of papers on learning, here is just a brief note
of his work on learning small strategies from MPSs for safety games using Angluin's algorithm.


```
NEIDER'S WORK ON LEARNING

Labeled safety games

  Vertices V0, V1
  Edges V0 × Σ × V1
  Strategies Σ* -> Σ

  Their Σ describes actions of both Adam and Eve

Strategy automata (Σ,Q,q0,δ,F)

  Describes the language of all possible plays
  compatible with some winning strategy.
  Note:
  there might be several different strategy automata,
  e.g. when they correspond to different winning strategies.

Canonical strategy automaton

  It is the strategy automaton describing the MPS and that is
  constructed using the states of the winning region of the game

Learning task

   input: a labeled safety game
  output: a strategy automaton DFA

Queries

  Membership query: return true iff w in L(MPS)
  Assuming that the MPS is input exhaustive,
  this makes sense for w ending with o in outputs.
  !To answer this query, you need to solve the game!

  Equivalence query on conjecture A:
  check (a) L(A)⊆ L(MPS) and (b) A is exhaustive for inputs.
  This can be done using the model-checking calls.

Learning algorithm

  (Recall
   consistency: v ~ u => vσ ~ uσ for all σ in Σ and v,u in R
   closedness: for all u in R and σ, [uσ] ~ [v] for some v in R.)

  Using membership queries, fill in the table until it is consistent.
  Drop the closedness assumption.

  Form a conjecture A: since the closedness is dropped,
  A can try to set δ([u],σ) = [uσ] but uσ is not in R so the state [uσ] is undefined:
  first, if there exists v ~ u with the state [vσ] defined then set δ([u],σ) = [vσ],
  but if that also fails, send it to the rejecting state. (Seems like an optimisation.)

  Hence for some σ, the function δ([u],σ) may lead to rejecting state.

  Make an equivalence query:

  If the check fails, output a counterexample w in Σ* such that (roughly)
  (a) w is in L(A)\L(MPS) ('w leads to a losing state').
  (b) A is not exhaustive for inputs.
  In both cases, proceed as usual: add all prefixes of w to R,
  and repeat.

How outputs are picked

  The algorithm cannot, by construction, pick a wrong output:
  for any v in R and s in Sep T(v·s)=1 means that the outputs
  of v and s are correct.
  (Recall they check membership using MPS.)
  The outputs are picked by the teacher when she provides
  a counterexample.

Caveat

  They mention a caveat specific to Angluin's learning algorithm,
  which leads to way too large learned automata,
  and show that Kearn and Vazirani's algorithm is superior
  (and it is on this they published a paper).
  I didn't read this closely.

```
