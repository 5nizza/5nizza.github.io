---
title: Complete Synthesis Procedure for TSL
layout: post
---

Regarding the logic, I just realised that TSL(=) formulas can always be translated into deterministic register automata:

- treat tests (i.e. the predicate "=") and assignments as letters from a finite alphabet (there are only finitely many of tests and assignments over a fixed number of variables, which come from the TSL(=) formula)

- convert the LTL formula over such letters into a deterministic parity automaton. This automaton works on "action words" (i.e., sequences of test-assignment letters)

- intersect this automaton with the "feasibility checker" automaton. Such an automaton tracks the relations between the "registers" (data variables used in the formula) and makes sure the partitions and the tests/assignments are consistent. (For instance, for two variables x and y, if the state has partition "x=y" then the test x!=y is not possible.)

- Finally, treat the resulting deterministic parity automaton as a det register automaton. The resulting automaton accepts a data word iff it satisfies the original TSL formula.

(The key in the correctness of this translation is that the original TSL formula talks explicitly about the assignments and the tests of the registers.)

Because synthesis from deterministic register automata is decidable, we have that synthesis from TSL(=) is decidable. I guess your procedure will always terminate in this case. (Also: since synthesis from det register automata over (N,<) is decidable [1], the same argument works for TSL(=,<). But that is probably the max you can get.)


[1]: https://arxiv.org/abs/2004.12141

