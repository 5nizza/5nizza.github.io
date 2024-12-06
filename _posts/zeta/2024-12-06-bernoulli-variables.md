---
title: "Bernoulli Variables"
tags: [statistics, ML]
layout: post
---

__Definition__:
Given $N$, variables $X_1,\ldots,X_N \in \\{0,1\\}$ are Bernoulli variables if they were drawn independently from some fixed distribution.

__Question__:
Suppose we uniformly-randomly pick a subset $D=\\{Y_1,\ldots,Y_N\\}$ of size $N$ from some fixed population set.
Then we sum up the numbers and:
- if the sum is larger than 5 then we set $X_1,\ldots,X_N = 1,\ldots,1$, and
- otherwise we set all $X$-variables to 0.

_Does the process "pick $D$, then pick $X_1,\ldots,X_N$" yield Bernoulli variables?_

__Answer__:
No, the resulting variables $X_1,\ldots,X_N$ are not Bernoulli variables
since they are not independent from each other,
i.e.,
for instance, $p(X_2=0 \mid X_2=0) = 1 \neq P(X_1=0) \cdot P(X_2=0)$.

__Consequence__:
The Hoeffding's inequality allows for bounding the generalization error, as follows.
Suppose $X_1,...,X_N$ are Bernoulli random Boolean variables (taking values 0 or 1).
Then the Hoeffding's inequality says:
<br/>
$$
P(|M - E(M)| > \epsilon) < 2 e^{-2|N|\epsilon^2}
$$
<br/>
where $M = \frac{1}{N}(X_1+\ldots+X_N)$ is the mean average of $X_1,\ldots,X_N$, and $E(M)$ is the expected mean average
(i.e., we pick $X_1,\ldots,X_N$ many-many times, write down their mean, and then $E(M)$ is the mean of those means).

For a single hypothesis $h$, we can then derive:
<br/>
$$
P(|mErr(h) - mErr(h,D)| > \epsilon) < 2 e^{-2|D|\epsilon^2}
$$
<br/>
where $mErr(h)$ is the mean error on the whole population.

The temptation is to use exactly the same equation for the case of two hypothesis, $h_1$ and $h_2$.
The reasoning would go as follows.
Uniformly-randomly pick a subset $D$.
We don't know beforehand what $g$ will be, $h_1$ or $h_2$ (the better-performing one on $D$).
However, since $h_1$ and $h_2$ are fixed, there exists a probability $p(h_1)$ of picking $h_1$ and $p(h_2)$ of picking $h_2$ (note: $p(h_1) = 1-p(h_2)$).
To use the inequality,
we need to show that $err(g,x_1),err(g,x_2),\ldots,err(g,x_N)$ are Bernoulli variables,
where $g$ depends on the choice of $D=\\{x_1,\ldots,x_N\\}$.
That is however exactly where the problem lies:
the dependendence of $g$ on $D$ implies that
$err(g,x_1), err(g,x_2), \ldots,err(g,x_N)$ depend on each other,
because $err(g,x_i) = err(h_1,x_i)$ implies $err(g,x_{i+1}) = err(h_1,x_{i+1})$.





The correct generalization bound for the case of several hypothesis $H$ is:
<br/>
$$
P(|mErr(h) - mErr(h,D)| < \epsilon) > 2 |H| e^{-2|D|\epsilon^2}
$$
<br/>

