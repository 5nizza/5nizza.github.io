---
layout: post
title: On Reif's algorithm for games of incomplete information
---

A note on Reif's paper "Complexity of two player games of incomplete information"
[^Reif84].
I found the definition of games of partial information unusual.
It has two axioms, without which Reif's algorithm is wrong.
Below I describe the definition and what happens 
if we take away the second axiom.

The setup is: a game $$G=(POS_0, POS_1, \rightarrow)$$, with 
$$POS_0 \cap POS_1 = \emptyset$$, and every position of $$POS_{0/1}$$ have
private information to the env (player 0), 
thus moves of the system (player 1) should not depend on it.
We also require the following two axioms to hold:

1. _If $$p \in POS_1$$ and $$p \rightarrow p'$$, 
   then $$priv_0(p) = priv_0(p')$$_.
   I.e., when system moves, 
   the private to env information does not change.

2. _If $$p,q \in POS_1 \setminus W$$ and $$vis_1(p)=vis_1(q)$$,
   then $$\{vis_1(p') | p \rightarrow p' \}$$ 
   and  $$\{vis_1(q') | q \rightarrow q' \}$$._
   I.e., 
   for any two states, if their observations equal,
   then any subsequent states will have the same observations.

The second axiom surprised me.
At first, I missed it, 
and conluded that Reif's algorithm is wrong.
The puzzle was resolved by my colleague, who pointed to this axiom.

The goal for a player is to drive the opponent into a state with no successors, 
thus the last who made a move wins!
This is similar to the reachability objective.

Why is the axiom important?
Below I show that without this axiom the algorithm is "buggy".

Reif's algorithm decides if the game has the winning strategy for the system.
The algorithm is to be run on alternating TMs.

__Excurse into alternating TMs__.
An alternating TM has universal and existential "choose" commands.
Recall that a nondeterministic TM accepts an input if its computational tree
contains a path which is accepting:

            (E)
            / \
          (E)  \
          /\   ...
         /  \
     accept  reject

Where `E` means "existentially choose" (or "guess" the move).
The acceptance condition rephrased: 
the top node should evaluate to `true`,
where any `accept` evaluates to `true`, and `E` node evaluates to `true` 
iff there is a child node that evaluates to `true`
(as you would expect).
The alternating TM extends the non-deterministic TM with the command
`A` "universally choose",
i.e., nodes of type `A` ("universally choose").
As before, for a computational tree to be accepted 
the top node should evaluate to `true`:

            (E)                   (E)   
            / \                   / \   
          (A)  ..               (A) rej 
          /\                    /\      
         /  \                  /  \     
       acc  rej              acc  acc   
                                        
      (rejecting)           (accepting)  

Now to the main part.


__Reif's algorithm__. 
For a state $$p$$, denote by $$vis(p)$$ the information visible to player 1
(and player 0 sees everything).

~~~ python
# INPUT: game (POS1, POS2, ->), initial state p_initial
P = {p_initial}
while True:
    P` = {p` | p->p`, p in P}
    W(P) = {p in P | p has no successors}
    V` = {vis(p`) | p` in P`}
  
    if P is a subset of POS1:
        if W(P) is not empty: 
            return REJECT                     
        v = "existentially choose" an element of V`   # L1
    else:  # P is a subset of POS2
        if W(P) == P: 
            return ACCEPT
        v = "universally choose" an element of V`     # L0
    
    P = {p` in P` | vis(p`) = v}                      # L3
~~~

The algorithm checks, step by step, 
if the system can force the env into the bad state,
and if the env can force the system into the bad state.

Then:

- `P` represents the set of current states we can be in,
  provided the visible history until the current step
- we interleave between moves of player 1 and 2 
  (`P is a subset of POS1`/`P is a subset of POS2`)
- line `L1`/`L0` represents the system/environment move
- in line `L3` we calculate possible states of the env
  provided the visible state is `v`.
  In a sense, this is what the sys can know.

__Where is the "bug"?__
The line `L1` looks wrong (if not account for the axiom).
Consider the game graph below with `[..]` states belonging to the env,
and `(..)` -- to the system, and `..` denotes env's private info.
    
           (.., c) --> [.., c2] DEAD
          /   cT
    [init]
          \
           (.., c) --> [.., c3] --> (..) DEAD
              cB            

In the game the env wins (it goes down in the first step).
Execute the algorithm:

1. `P={init}`, `P'={cT,cB}`, `V'={c}`, executing line `L0` gives `c` 
   (the only possibility).
2. `P={cT,cB}`, `P'={[..,c2], [..,c3]}`, `V'={c2,c3}`, executing line `L1`.
   We "extistentially choose" `c2` out of `{c2,c3}`. Oh-oh.
3. `P={[..,c2]}`, `W(P)=P=[..,c2]` and we return `ACCEPT` on this computational path.
   But this is the only computation path, so we return `ACCEPT` meaning 
   the game is won by the sys.

Below is the computation tree for this game:

        (A)
         |
        (E)
        / \
      acc rej

which evaluates to `true`.

__Lesson learned: whenever found a "bug", read the definitions__.

__Notes__

- The algorithm does not necessary terminate: consider games with cyclic graphs
  (but that looks like a minor problem -- replace `while` loop with `for` loop
   that counts up to "the number of states")


__Handy material__

References to "beginner" material on imperfect-information games 
for studying would be handy! 
Here are some refs on games in general:
(without links but you can even find downloadable versions):

  - "Automata Logics, and Infinite Games: A Guide to Current Research" -- 
    for advanced students?
  - "Lectures in Game Theory for Computer Scientists" (by K.R. Apt, E. Graedel) -- 
    looks promising
  - "Game Theory in Formal Verification" (lectures by Krishnendu Chatterjee at IST)
    (no imperfect information games)
  - "Infinite Games" by Martin Zimmerman and Felix Klein (Saarland Uni) -- 
    very good one, no imperfect information games



#### Footnotes
[^Reif84]: _Complexity of two-player games of incomplete information_, 
         John H. Reif, 1984
