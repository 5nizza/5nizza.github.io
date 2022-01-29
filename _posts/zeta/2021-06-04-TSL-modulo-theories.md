---
title: "Temporal Stream Logic modulo Theories"
tags: logic data SMT
layout: post
---


[Link](https://arxiv.org/abs/2104.14988)

The paper studies satisfiability algorithms of Temporal Stream Logic extended with Theories.

The number of (memory) cells is fixed a-priori.
Inputs and outputs are data, _but_ they all will be treaten syntactically in the synthesis approach.
'TSL' specifications contain function and predicate terms, and updates.
You can't compare data for equality, etc., only what is permitted by the (uninterpreted) predicates.
The grammar of TSL formulas is:\
$
\varphi ~~ := ~~ true ~~ \| ~~ \neg \varphi ~~ \| ~~ \varphi \wedge \varphi ~~ \| ~~ X \varphi ~~ \| ~~ \varphi U \varphi ~~ \| ~~ \tau_p ~~ \| ~~ c \gets \tau
$\
where $\tau_p$ is a predicate and $c \gets \tau$ updates cell $c$ with the current value of the function term $\tau$.

Their results:

- First, note that the logic is undecidable (not semi-decidable nor co-semi-decidable)
- They describe some semi-decidable subsets
- The paper also shows undecidability of the logic extended with equalities, or with Presburger arithmetic.
  However note that in both cases, UFs are allowed as well!
- They introduce the notion of Buchi Stream automata.
  Such automata use _guards_ and _updates_ on the transitions.
  They show there is an algorithm to convert any formula to such an automaton.
  (which is interesting, actually, because I there is no such complete translation in the related case of register automata and logic with variables)
- They also provide an [implemtation][1]: the tool uses SPOT for coversion (some kind of syntactic conversion).
  They have (seemingly simple) [benchmarks][2].

See also logics: $FO2(\equiv, >, +1)$, LTL with freeze quantifier, VLTL (but they don't study satisfiability -- only MC), Constraint LTL and LRV.


[1]: https://github.com/reactive-systems/tsl-satisfiability-modulo-theories/
[2]: https://github.com/reactive-systems/tsl-satisfiability-modulo-theories/tree/master/benchmarks
