---
title: Note on Estimator-Based Reactive Synthesis under Incomplete Information
layout: post
status: draft
---

A short note on [Estimator-Based Reactive Synthesis](http://motesy.cs.uni-bremen.de/pdfs/hscc2015.pdf).
I was looking for complexity results of computing memory-less strategies 
under incomplete information, and stumbled upon this paper.

The popular practical synthesis method, for GR1 specifications, 
is PTIME (wrt. game arena size).
In practice we often get incomplete information about the surroundng,
for example:
a robot may not have a full info about the surrounding,
or its sensors may be faulty/noisy.
Unfortunately, the complexity of the reactive synthesis under incomplete information
is EXPTIME, which is often impractical.
Can we get PTIME?

In the paper, the authors suggest to loose completeness at the cost of PTIME procedure.
But the degree of lost completeness is somewhat "manageable" by designers:
designers specify what information should be tracked from the surroundings
(thus, we use human expertise).

In other words, in the EXPTIME procedure, we automatically track as much as possible.
If something can be learned, we learn it.
In the proposed procedure, designers restrict what should actually be tracked.

The synthesis procedure is divided into two steps: 

1. synthesize a _positional estimator_
2. synthesize the full system provided the estimator

Both steps use the synthesis under full information, and are PTIME 
(they use GR1 procedure).
Both steps are complete.
But..be careful to understand what is a _positional estimator_!
At the first reading, I misuderstood the definition,
and did not believe into the completeness.

Here is the definition of an _estimator_ 
(definition of the "positional" follows) from their paper:

<img src="{{ site.url }}/assets/estimator-def.png" width="500px"/>

Note that $$\rho_e \rightarrow_s \rho_s$$ means $$\rho_s W \neg \rho_e$$.
Intuitively, an estimator should satisfy safety-like spec,
and be independent of hidden values.

Now comes the interesting part:

<img src="{{ site.url }}/assets/positional-estimator-def.png" width="500px"/>

Note:

- when gluing sequences of hidden valuations, 
  the values of hidden variables may jump at the "mixing point"!
  But the _positional_ estimator should tolerate such jumps,
  and still satisfy $$\rho_s$$.
  I don't have an intuition for that yet
  (does this imply that hidden values can jump _all the time_?)

This is important -- I missed this step at first.

The proof of completeness of the first step uses this fact.
I put it here, but be careful, it might be junk.

~~~

Computing positional estimators via rho_u is complete.
>>
Suppose we miss a PE strategy.
⇔
there is a PE strategy, but it is not included into rho_u
⇔
at least one of its moves is not included into rho_u

thus, let (o,e)->(o',e') \not\in rho_u
⇔
either
(a)      ∀h:¬R(o,e,h) or
(b)  ∃h1,h2: R(o,e,h1) &
             (o,e,h1)->(o',e',h2) |= ¬(rho_e->rho_s)

(a) is not possible
(b) consider R(o,e,h1) & (o,e,h1)->(o',e',h2) |= ¬(rho_e->rho_s)
    ⇔
    R(o,e,h1) & (o,e,h1)->(o',e',h2) |= rho_e & ¬rho_s

    If use the original strategy, is R(o,e,h1)=true?
    1. R(o,e,h1)=true
       Acc. def1 ("seq of hidden values does not affect strategy's truth of rho_e->rho_s"):
         (o,e,h1)->(o',e',h2) |= rho_e -> rho_s,
       but we know that (o,e,h1)->(o',e',h2) |= rho_e & ¬rho_s
       ϟ

    2. R(o,e,h1)=false
       It is not possible to reach (o,e,h1) if play acc. the original strategy.
       But there is h` s.t. (o,e,h`) is reachable if play acc. the original strategy [^1].
       Let r` be the sequence of hidden values with which (o,e,h`) is reachable (at moment i`):
         r` = r`_0...r`_i`... where r`_i`=h`
       We also know for h1 and h2:
         R(o,e,h1) & (o,e,h1)->(o',e',h2) |= rho_e & ¬rho_s
       Let r be the sequence of hidden values with which (o,e,h1) is reachable (at moment i),
       followed by (o,e,h2) at moment {i+1}.
       Acc. def2 PE strategy should satisfy rho_s if plaid on the composed sequence
         r`_0...r`_{i-1}` r_i r_{i+1}
       Consider the transition on (r_i,r_{i+1}):
         acc. the original strategy we play (o,e)->(o',e')
         ⇔
         (o,e,r_i) -> (o',e',r_{i+1})
         ⇔
         (o,e,h1) -> (o',e',h2)
       Acc. def2, since our strategy is PE, (o,e,h1)->(o',e',h2) should satisfy rho_s,
       but we know that
         R(o,e,h1) & (o,e,h1)->(o',e',h2) |= rho_e & ¬rho_s
       ϟ
⇔
the assumption (that we miss a PE strategy) is false.

<<
Footnotes:
[^1]:  in the beginning, we should consider only such (o,e)->(o',e')..
~~~

