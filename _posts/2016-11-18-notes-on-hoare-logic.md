---
title: Notes on Hoare Assignment Axiom
layout: post
---

$\small\textit{"The obvious things are the most difficult to understand"}$

I haven't seen the intuitive explanation of the Hoare assignment axiom
(while the Floyd axiom have an intuitive explanation).
Below I tried to "convince myself" that the Hoare assignment axiom makes sense.

The assignment axiom is   
$$
\{ P \} v:=expr \{ Q \}
$$   
where $P=P(v, others)$, $Q=Q(v,others)$, and $expr=expr(v,others)$
are boolean formulas over integer (or other domains) variables.
Let's shorten $others$ to $o$ onwards.

What is the largest $P$ such that:
whenever it holds before the assignment, then $Q$ holds after the assignment?
The Hoare axiom for the assignment says $P=Q(expr(v,o), o)$.
Why?

The first direction $\Rightarrow$.
Assume that $Q(expr(v,o), o)$ holds before the assignment $v:=expr(v,o)$.
Then, obviously, $Q(v_1, o)$ also holds where $v_1=expr(v,o)$
(i.e., the updated $v$).
Thus, if originally $expr(v,others)$ and $others$ satisfy $Q$,
then after the assignment (replacing $v$ by $expr$) $Q$ will also hold.

(The other direction)
Let's see that
$P=Q(expr(v,o), o)$ is the largest such $P$.
Assume there is $P'$ such that for some $v$ and $others$,
$P'(v,o) \land \neg Q(expr(v,o))$ holds before the assignment,
and after the assignment $Q$ holds.
The latter means $Q(v_1,o)$ where $v_1$ is the updated value of $v$,
i.e., $v_1=expr(v,o)$,
and thus we have that $Q(expr(v,o), o)$ holds.
This contradicts $\neg Q(expr(v,o),o)$ (that we assume we have).
Thus, $Q(expr(v,o),o)$ is the largest such $P$.
