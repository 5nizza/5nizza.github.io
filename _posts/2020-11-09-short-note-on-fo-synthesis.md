---
title: Undecidability of synthesis from FO with data
layout: post
---

Just a note for myself to remeber.
In paper 'Synthesis with Identifiers' Ruediger Ehlers writes that
finding a logic that fits well register transducers with equality tests is hard.
To justify, he shows that the specification of request-grant arbiter needs _three_ variables
and _binary_ relations:  
$\forall x,d. req(x) \land data(x,d) \rightarrow \exists y. y>x \land grant(y) \land data(y,d)$,  
where $data(x,d)$ means that the data at position $x$ is $d$, and the predicate $>$ needs a separate defintion:

- total: $\forall x,y. (x>y) \vee (x=y) \vee (y>x)$;
- antisymmetric: $\forall x,y. x>y \rightarrow \neg (y>x)$;
- transitive: $\forall x,y,z. x>y \land y>z \rightarrow x>z$.

Thus, it _seems_ like the required fragment of FO to specify the request-grant arbiter
is of the form $(\forall x,y,z. \varphi) \land (\forall x,d.\exists y.\psi)$
and where $\psi$ uses binary predicates.
Then, since satisfiability of this fragment is undecidable,
he concludes that synthesis is undecidable as well.

However, there is an alternative way to specify the request-grant arbiter:  
$\forall x. req(x) \rightarrow \exists y. y>x \land grant(y) \land x \equiv y$,  
where the equivalence relation $\equiv$ and the linear order relation are _given_.
Since these relations are given, we do not need to axiomatise them, and
the whole formula uses only two variables.
Such logics were very well studied and denoted $FO^2(<,\equiv)$;
note that you should be careful about whether $<$ is a successor relation (aka 'next element') <!---->
or it is a linear order.
(Another note: you cannot specify succ using linear order in $FO^2$, you will need three variables).

Conclusion:
the quest of decidable fragments of logic with data that can specify the arbiter is open!


