---
title: Quick notes on submitted tools for SYNTCOMP
layout: post
---



# SDF

Implementation of the fix-point computation for safety games.
Plain implementation without any new optimizations:

- computing "transition function":
  the transition function is saved in the form of a map (this is standard)
  latch -> BDD for that latch
  Thus, there are no variables for primed latches

- computing "transition function": 
  SDF caches BDDs for computed nodes of AIGER.
  Despite CUDD having its own cache,
  this really helps on amba_match examples.

- computing the predecessor:
  SDF uses the direct substitution CUDD's command (standard), i.e.:
  ∀u ∃c: (!error(t,u,c)  &  (dst(t)[t <- bdd_next_t(t,u,c)]))

- during win_region computation:
  (the following does not seem to help)
  each time re-ordering happens,
  SDF saves the resulting ordering of variables.
  After some time limit, SDF groups variables appearing often together.
  This speeds up reordering, but the decreases its quality.

- before computing the strategy,
  SDF removes all BDDs except the nondeterministic strategy,
  and then calls reordering SIFT_CONVERGE

- strategy determinization (using co-factor approach):
  SDF does "variables elimintation"
  (this is also standard):
  when checking a strategy for a variable,
  it checks for all other variables if they can be safely removed
  (i.e., check if function for controllable_a can be independent of other variables)

- translating into AIGER:
  caching computed AIGER circuits,
  i.e., SDF caches AIGER nodes for each BDD node it computes,
  thus if two BDDs share a BDD node -- it will be shared in AIGER as well.
  (significantly reduces the circuit size)
  No ABC optimizations are used.



# PARTY-ELLI(for rally)

Implementation of the bounded synthesis approach originally by Schewe Finkbeiner:

- LTL formula -> negated formula -> NBW -> treat it as UCW
- fix system S size k
- encode S*UCW into the SMT query which is SAT iff there exists system of size k
  that realizes the spec
- if UNSAT, increase the size
- if SAT, turn into aiger:
  verilog -> mv [using vl2mv] -> aiger [using ABC with ABC optimizations]

Differences to the standard SF approach:

- systems are not input preserving
- formula strengthening:

    G(...) & GF(..) -> G(...) & GF(..)

  is rewritten into

    G(...) W !G(...) &
    G(...) & GF(..) -> GF(..)

  where G(...) is a safety formula that starts with G,
  GF(..) - liveness formula.
- when the spec automaton has two SCCs,
  then there is no relation between the counter values of states of those different SCCs

Before checking the SAT I check if the spec is UNSAT:

- negate formula, change inputs and outputs, check for 'system' of size 1
- the check is time restricted to ~200 seconds.

Notes:

- formula strengthening can be seen as 'cheating',
  but the tool never returns unsound answer
  (if rewritten formula is found UNSAT,
  then the tool searches for a strategy for the original formula)
- formula strengthening is crucial for performance
- use: python, Z3 (logic LRA, incremental mode), ABC, vl2mv


