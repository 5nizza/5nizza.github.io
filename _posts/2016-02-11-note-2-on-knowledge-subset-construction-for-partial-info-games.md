---
title: Some Explanation of the Proof of Lemma 4 from Algorithms for Omega-Regular Games with Incomlete Information
layout: post
---

This post is about the proof of Lemma 4 from [Algorithms for Omega-Regular Games with Incomplete Information](http://www.eecs.berkeley.edu/Pubs/TechRpts/2006/EECS-2006-89.pdf).   
Finally, I understoot it.   
The lemma states:

- $\exists \alpha^o \forall \beta: play_G(\alpha^o,\beta) \models \varphi
  \Rightarrow
  \exists \alpha^k \forall \beta: play_{G^k}(\alpha^k,\beta) \models \varphi$

The authors prove it by contradiction:
assume $\alpha^o$ wins in $G$, but any $\alpha^k$ looses in $G^k$ and thus
there is $\beta^S$ that wins in $G^k$,
where $\beta^S$ is a knowledge-based strategy for $\beta$-player.
We will construct a play in $G$ where $\alpha^o$ plays acc. $\alpha^o$,
and $\beta$ plays acc. $\beta^S$-inspired strategy.
Then show that such play satisfies the objective $\varphi$ and does not satisfy it,
which is the sought contradiction.

To build the play (that adheres to $\alpha^o$ and $\beta^S$),
we first build the following tree $T(\alpha^o,\beta^S) = (N,\Sigma,\Delta_T)$:

- any node $\rho \in N$ will be from $Pref(G)$,
  i.e., a node is of the form $l_0 \sigma_0 l_1 \sigma_1 ... l_n \sigma_n$.

- for any two nodes, $\rho$ and $\rho \sigma l'$,
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

The authors define the tree in 'batches':
they define $N_i$ and set $N = \bigcup_i N_i$,
and also $E_i$ and set $E= \bigcup_i E_i$.
Intuitively, every set $N_i$ of nodes describes all possible prefixes of length $i$
when played acc. $\alpha^o$ and $\beta^S$.
Also, $E_i$ describes possible evolutions of prefixes of length $i$ into prefixes
of length $i+1$.    
The definition is inductive:

- initially, $$N_0 = \{ l_0 \}$$
- from $N_i$ let's build $N_{i+1}, E_{i+1}$:
  - $\sigma_i = \alpha^o(\rho)$
  - let $$s_{i+1} = \beta^S(obs(\rho))$$ for some $\rho \in N_i$,
    then $$N_{i+1} = \{\rho \sigma_i l' : \rho \in N_i \land l' \in s_{i+1} \land (Last(\rho), \sigma_i,l') \in \Delta \}$$
  - and $$E_{i+1} = \{ (\rho, \sigma_i, \rho\sigma_i l') : \rho \in N_i \land \rho\sigma_i l' \in N_{i+1} \}$$

Below is a picture:

<img src="{{site.url}}/assets/lemma4-tree-def.jpg" width="600px"/>

Note that the following properties holds:

(c) $\forall \rho, \rho' \in N_i: obs(\rho) = obs(\rho_i)$    
(a) and $$s_i = \{ Last(\rho) : \rho \in N_i \}$$      
(d) and $$s_i = K(obs(\rho))$$ for any $\rho \in N_i$       
(b) $N_i \subseteq Pref(G)$ and $s_0 \sigma_0 ... \sigma_{i-1} s_i \in Pref(G^k)$     

Also:    
(i) $s_{i+1} \neq \emptyset$    
(ii) $(s_i, \sigma_i, \sigma_{i+1}) \in \Delta^k$ and $s_{i+1} \subseteq Post(G, \sigma_i, s_i)$      
(iii) $s_{i+1} \subseteq obs$

The authors prove them by induction (I did not look at them really).   

Now to the main part:
this tree is infinite, because $s_{i+1} \neq \emptyset$ by (i) and (a)!
But it is also finitely-branching, because there is only a finite number
of states in $G$.
Hence, by Koenig's lemma there is an infinite path in the tree.   
From that path we can build an "infinite prefix", i.e., a play of $G$.   
This play satisfies the objective $\varphi$, because $\alpha^o$ is winning.
On the other hand, from this infinite play of $G$ we can build
an infinite play of $G^k$.
Such play adheres to $\beta^S$ and, thus, should violate $\varphi$.
Contradiction.
