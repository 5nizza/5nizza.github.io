---
title: "Lazy Synthesis"
tags: synthesis CEGIS LTL
layout: post
---

\

[Paper link](http://www.swenjacobs.com/publications/VMCAI12.pdf)

They consider the standard LTL synthesis problem of finding an implementation from a given LTL formula.
They have three phases, `SOLVE`, `CHECK`, `REFINE` (I think those are typical for CEGIS).

- initially the spec is `true`

`SOLVE`:
- Synthesise an impl that satisfies the spec (initially, any impl).
  They use SMT/SAT solvers for that (i.e. use 'bounded' synthesis).

`CHECK`:
- Translate the LTL formula to a safety automaton
  (formula -> UCW -> k-UCW, where k depends on the size of the impl)

- Check `impl |= safety_automaton`.
  For this, they use the BDD backward reachability search on the product of impl with the negation of safety\_atm.
  BDDs seem like an overkill for this simple nonemptiness check,
  however it is actually clever as you will see.

  `if impl |= safety_automaton:` we are done, return the impl   
  `else REFINE:`   
    - The BDD backward reachability search produces a witness which is _symbolic_ and has the form
    States0 States1 States2 ... StatesERR,
    where States0 contain the initial state of the product impl\*atm and StatesERR are error states
    (i.e. pairs (t,err) where t is an impl state and err is an accepting state of the negation of the safety atm).
    From the witness, construct a tree whose edges are labeled with inputs-outputs and
    nodes are the states of the product impl\*atm.
    The root state has to be the initial state of the product,
    and the leafs are the error states.
    Thus, each branch of the tree encodes the sequence of inputs and outputs
    that lead to a bad state, so the correct system should _not_ respond to the inputs
    of the branch with the outputs of the branch.
    We can encode all these counter-examples into SMT constraints.
    These new SMT constraints are added to the spec, and we start over again in `SOLVE`.

(Note: it seems that once you increase the bound on the size of the implementation,
 you have to discard all constraints and start the CEGIS loop from the scratch.
 This is because in the conversion UCW -> k-UCW the parameter k depends on the system size,
 and small k make the spec harder to realise.)

_My opinion on the approach_:  
They are trying to avoid converting the whole spec into a SAT query at once,
and instead "learn" an easier spec and hope to get lucky that the SAT solver finds
an implementation of an easier spec that realies the whole spec.
I am not sure this approach is more efficient than doing it all at once, but who knows, they don't compare:-)
What I do like is that using BDDs gives you a symbolic witness
(although you have to de-symbolise it by encoding into a tree).

(The paper also uses white-box and black-box processes and manages to synthesise AMBA.)

