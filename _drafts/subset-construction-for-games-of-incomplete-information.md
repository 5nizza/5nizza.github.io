---
title: the Subset construction for games of imperfect information
layout: post
status: draft
---

Some rewordings of one result from paper 
"Games with imperfect information: theory and algorithms"
(which is the tutorial version of "__TODO__").

The result we will look at is:

> Player 1 has a surely winning observational strategy 
> iff 
> player 1 has a surely winning strategy in the knowledge game.

The idea is:

- $$
  \exist \alpha^o \forall \beta: play(\alpha^o,\beta) \models \phi 
  \rightarrow
  \exist \alpha^k \forall \beta: play(\alpha^k,\beta) \models \phi
  $$:
  Given $$\alpha^o: Obs^+ \rightarrow \Sigma$$, 
  let's construct $$\alpha^k: K^+ \rightarrow \Sigma$$.
  Consider what to do at prefix $$k_0, ..., k_m$$.
  We can uniquely map this prefix to $$o_0, ..., o_m$$.
  So let's use the action $$\alpha^o(o_0,...,o_m)$$.
  Such strategy will be winning, because the original is.

- $$
  \exist \alpha^o \forall \beta: play(\alpha^o,\beta) \models \phi 
  \lefttarrow
  \exist \alpha^k \forall \beta: play(\alpha^k,\beta) \models \phi
  $$:
  Given $$\alpha^k: K^+ \rightarrow \Sigma$$, 
  we need to construct $$\alpha^o: Obs^+ \rightarrow \Sigma$$.
  Recall that the knowledge function is
  $$F_K: Obs^+ \rightarrow K$$ such that ... (see def).
  Let's construct $$\alpha^o$$ as follows.
  Suppose we are at prefix $$o_0, ..., o_m$$.
  This prefix can be uniquely mapped into $$k_0, ..., k_m$$,
  because each $$k_j$$ is uniquely computed from $$o_0, ..., o_j$$.
  So let's use $$\alpha^o(o_0, ..., o_m) = \alpha^k(k_0, ..., k_m)$$.
  The strategy $$\alpha^o$$ is winning, because $$\alpha^k$$ is.

__TODO__: i am not sure the arguments are correct. 
      Are these strategies well-defined?
      Are they winning?


#### Notes

- the theorem talks about player 1 only -- does the result holds for player 2?
- how to compute strategies?
