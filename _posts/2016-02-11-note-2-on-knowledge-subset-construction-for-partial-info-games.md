---
title: Note on Proof 4 from Algorithms for Omega-Regular Games with Incomlete Information
layout: post
---

This post is about the proof of Lemma 4 from [Algorithms for Omega-Regular Games with Incomplete Information](http://www.eecs.berkeley.edu/Pubs/TechRpts/2006/EECS-2006-89.pdf).   
Finally, I understood it.   
The lemma states:

 $$\exists \alpha^o \forall \beta: play_G(\alpha^o,\beta) \models \varphi
  \ \ \ \Rightarrow \ \ \ 
  \exists \alpha^k \forall \beta: play_{G^k}(\alpha^k,\beta) \models \varphi$$

Below is a slightly different version of the proof.

#### High-level proof idea
The authors prove it by contradiction:
assume $\alpha^o$ wins in $G$, but any $\alpha^k$ looses in $G^k$ and thus
there is $\beta^S$ that wins in $G^k$,
where $\beta^S$ is a knowledge-based strategy for $\beta$-player.
We will construct a play in $G$ where $\alpha^o$ plays acc. $\alpha^o$,
and $\beta$ plays acc. $\beta^S$-inspired strategy.
Then show that such play satisfies the objective $\varphi$ and does not satisfy it,
which is the sought contradiction.

#### Construction of the play intuitively
How to build such a play?
Imagine two games, $G$ and the corresponding $G^k$.
In both games we start in the initial state
(in $G$ it is called $l_0$, in $G^k$ it is called $s_0 = \\{l_0\\}$).
From $l_0$, in $G$, $\alpha^o$ chooses the next action $\sigma=\alpha^o(l_0)$.
Now switch to $G^k$:
for $s_0$ and the action $\sigma$, $\beta^S$ responds with
$s_1=\beta^S(s_0 \cdot \sigma)$.
For $G$, this means that we haven't completely fixed the reponse of $\beta$ --
we fixed only the next observation, but not the exact state (it can be any from $s_1$).
This way we proceed:
at any moment,
we know the current state $s_i$ in $G^k$
which is also a set of current states we can possibly be in $G$,
and we also know the observational history of a current state in $G$
(that is needed by $\alpha^o$;
 for any state (from $s_i$) that we can currently stay in $G$
 the history will be the same).   
The infinite sequence of observations produced
mhis way should not satisfy $\varphi$ because it is played acc. $\beta^S$,
and, on the other hand, it should satisfy $\varphi$ because we played acc. $\alpha^o$.
That is the sought contradiction.


#### Construction of the play formally

To build the play (that adheres to $\alpha^o$ and $\beta^S$),
we first build the labelled directed acyclic graph
$D(\alpha^o,\beta^S) = (N,\Sigma,\Delta_D)$.
This graph will encode all possible plays in $G$ that
adhere to $\alpha^o$ and to $\beta^S$
(the second adherence is for the corresponding play in $G^k$).
Nodes are of the form $(l,i)$ with $l \in S$ and $i \in N$.

<!--
Edge $((l,i), \sigma, (l',i+1)) \in \Delta_D$ iff
$obs(pref)$

and $\rho \sigma l'$,
  we will have $(\rho, \sigma, \rho\sigma l') \in \Delta_T$
  only if $(Last(\rho), \sigma, l') \in \Delta$     
  (later, from an infinite path $\rho_1 \rho_2 \rho_3 ...$
   of the tree we will build a play of $G$,
   by simply taking $Last(\rho_1)\sigma_1 Last(\rho_2) \sigma_2 ...$).

(
Aside note: Why do we build a tree, at all?
I think that by doing so, we can later use Koenig's lemma to justify
there is a play we searching for.
Recall, Koenig's lemma states: if a tree is infinite and finitely-branching,
then it has an infinite path.
And such path (from which we construct a play (which should be infinite by def.))
is what we are looking for.
The 'tree-story' allows us to find an _infinite_ path.
)
-->

We define the DAG in 'batches' starting with all the nodes that have $i=0$:
then nodes that have $i=1$, and edges between 0- and 1-nodes, and so on.
For each $i$, we define $s_i$, and set $N = \bigcup_i s_i$;
for each $i>0$, we define $E_i$
($E_i$ will be the edges between $i-1$ and $i$ nodes),
and set $E = \bigcup_i E_i$;
we also define helper $obs_i$.   
For $i=0$: $s_0 = \\{ l_0 \\}$ and $obs_0 = obs(l_0)$.   
Now proceed inductively:   
Given $s_i$, $obs_0, ..., obs_i$, $\sigma_0, ..., \sigma_i$, 
$E_0, ..., E_i$,
let's define $s_{i+1}$, $obs_{i+1}$, and $E_{i+1}$:

- $\sigma_i = \alpha^o(obs_0 obs_1 ... obs_i)$
- $s_{i+1} = \beta^S(s_0 \sigma_0 ... s_i \sigma_i)$,
  and $obs_{i+1}$ is the corresponding observation
- $E_{i+1}$: $\Delta_D$ consists of all $(l_i, \sigma_i, l_{i+1}) \in \Delta_G$
  where $l_i \in s_i$, $l_{i+1} \in s_{i+1}$

Let's see what properties hold for the DAG:

- $s_{i+1} \neq \emptyset$.
  This follows from the definition of states in $G^k$
  (recall $s_{i+1} \in S^k$).
- $\forall l_{i+1} \in s_{i+1}. \exists l_i \in s_i$: 
   $l_i, \sigma_i, l_{i+1}) \in \Delta_G \cap \Delta_D$.
  In words:
  any state of $s_{i+1}$ is reachable from some state of $s_i$ with $\sigma_i$.
- $s_0 ... s_i$ is a prefix of some $play_{G^k}(\alpha', \beta^S)$,
  and satisfies $\neg \varphi$

Now to the main part: our DAG can be seen as an infinite sequence $s_0 s_1 ...$ [^1],
which is also a play in $G^k$.
We need to prove that there is an infinite path in $G$:

- the DAG has an infinite number of nodes,
  and from every node there is an edge to the predecessor,
  hence we can build an infinite number of prefixes of $G$
  where $\alpha$ plays acc. $\alpha^o$.
  Let's show there is an infinite path in $G$[^2]:

  - start with $l_0$ ($s_0 = \\{l_0\\}$).
  - through nodes in $s_1$ $s_1$ goes an infinite number of paths.
    But $s_1$ is finite, hence there is $l_1 \in s_1$ through which
    goes an infinite number of paths.
    Take $l_1$.
  - any such path from $l_1$ goes through $s_2$, $s_2$ is finite,
    hence there is $l_2 \in s_2$ through which an infinite number
    of paths passes. Take such $l_3 \in s_3$.
  - as you see, we can inductively define $l_{i+1}$ from $l_i$:
    since $s_{i+1}$ is finite, and an infinite number of paths
    go through it, there is $l_{i+1}$ through which an infinite
    number of paths passes. Use it.


This way we can build path of $G$ inductively:
note that any $l_i$ is defined, thus we described an infinite play of $G$.
This play is played acc. $\alpha^o$ and thus should satisfy $\varphi$.

Finally, we built two plays, one in $G^k$, and one in $G$,
that exhibit the same sequence observations,
and, on one side, satisfies $\varphi$, and on the other -- vioalates $\varphi$.
Thus, we derived the sought contradiction.

Footnotes:

  [^1]:  because the DAG contains an infinite number of nodes
  [^2]:  the proof essentially repeats that of Konig's lemma

