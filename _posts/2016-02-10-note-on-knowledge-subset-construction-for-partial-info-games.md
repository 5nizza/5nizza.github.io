---
title: Note on the Knowledge-based Subset Construction for Solving Games with Incomplete Information
layout: post
---

This post is about Lemma 4 from [Algorithms for Omega-Regular Games with Incomplete Information](http://www.eecs.berkeley.edu/Pubs/TechRpts/2006/EECS-2006-89.pdf).
The paper contains (among others) two proofs:

- $\exists \alpha^o \forall \beta: play_G(\alpha^o,\beta) \models \varphi
  \Rightarrow
  \exists \alpha^k \forall \beta: play_{G^k}(\alpha^k,\beta) \models \varphi
  $

- $\exists \alpha^k \forall \beta: play_{G^k}(\alpha^k,\beta) \models \varphi
  \Rightarrow 
  \exists \alpha^o \forall \beta: play_G(\alpha^o,\beta) \models \varphi
  $

This post is about the first proof -- the second one is easier.

The original proof of the first item proceeds like this:
assume the left side $\exists \alpha^o ...$,
and,
to prove by contradiction,
assume the knowledge game is loosing for $\alpha$-player,
and thus
$\exists \beta^s \forall \alpha^k: play(\alpha^k,\beta^s) \not\models \varphi$.
Then, in $G$ 'build' a path $(\alpha^o,\beta'^s)$,
where $$\beta'^s$$ is 'derived' from $$\beta^s$$,
and, finally, show that the path does not satisfy $$\varphi$$,
which contradicts to the fact that $$\alpha^o$$ is a winning strategy.

I got confused by this proof (Lemma 4)
-- why do we need the Koenig's lemma? Isn't the tree actually a DAG? --
so came up with an alternative proof.
Disclaimer -- it might be wrong (please comment if you see an error).

Before we proceed, let me give an intuition behind the definition
of knowledge sets.
The knowledge game $G^k$ is defined as $(K,\Sigma,\Delta^k)$,
with states $K \subset 2^S$ such that any $k \in K$ is also $k \subseteq obs$
for some $obs \in Obs$.
The knowledge state intuitively defines knowledge of where
$\alpha$-player can be now in the game.
In other words, the current knowledge set contains all and only the states
that into which the $\beta$-player can enforce the play
provided the same history of observations.
Let's fix the strategy $\alpha^k$
(strategy of $\alpha$-player in game the knowledge game $G^k$).
Then the following property holds:   

 if $k_1 \sigma_1 ... k_n$ is a prefix of some $play_{G^k}(\alpha^k, \beta)$,   
 then    
 $\forall s_n \in k_n \exists s_1 \in k_1 ... s_{n-1} \in k_{n-1}: s_1 \sigma_1 ... s_{n-1} \sigma_{n-1} s_n$ is a prefix in $G$.

See also the [picture]({{ site.url }}/assets/property-knowledge-sets.jpg).
We will use this property when proving the first item.

Now back to the proof of the first item.
The idea is, having $$\alpha^o$$,
to construct $$ \alpha^k $$ to win the game $$ G^k $$.

1. Define $\alpha^k(k_1, ..., k_n) = \alpha^o(obs_1, ..., obs_n)$.
   Thus, $\alpha^k$ uses memory.
   Note that the value of $\alpha^k$ for any $k^+$
   is uniquely defined from $$\alpha^o$$'s value for the corresponding $$obs^+$$.

2. $$ \alpha^k $$ is winning in $$ G^k $$,
   i.e., 
   $$ \forall \beta: play(\alpha^k, \beta) \models \varphi $$.

   - Consider arbitrary prefix $$ k_1 \sigma_1, ..., k_n \sigma_n, k_{n+1}$$
     of arbitrary $$play(\alpha^k, \beta)$$ in $$ G^k $$.

   - Show the sequence $obs_1 \sigma_1, ..., obs_n \sigma_n, obs_{n+1}$
     is the prefix of some $play(\alpha^o, \beta')$
     (where $obs_i$ are defined from $k_i$).
     The question here is:
     does the sequence represent a valid prefix?
     I.e., is it really possible to transit in $G$ as the sequence describes?
     Assume it is not; then there is the earliest
     transition $$ k_n \sigma_n k_{n+1} $$ in $$ play_{G^k}(\alpha^k, \beta) $$
     for which $$ obs_n \sigma_n obs_{n+1} $$ is not possible
     in any $$ play_{G}(\alpha^o, \beta') $$.
     The definition of knowledge sets
     requires that $$ k_{n+1} = post_{G}(k_n, \sigma_n) \cap obs_{n+1} $$.
     Note that $$ k_1 \sigma_1, ..., k_n \sigma_n, k_{n+1} $$
     has the following property: 
     for any $$ s_{n+1} \in k_{n+1} $$
     there exists a way for $$\beta$$-player to play,
     and there exists $$s_1 \in obs_1, ..., s_n \in obs_n $$
     such that $s_1 \sigma_1, ..., s_n \sigma_n, s_{n+1}$
     is a valid prefix in $G$
     (this needs a separate proof,
      but intuitively that follows from the definition of the knowledge sets).

    - Finally, since any such sequence is a prefix of the winning sequence in $G$,
      the original sequence in $G^k$ should also be part of the winning sequence.
      Hm, there is a catch here: before we talked about finite prefixes,
      but now we should somehow conclude about infinite sequences.

What do you think -- do we really need Koenig's lemma as the original proof uses?

