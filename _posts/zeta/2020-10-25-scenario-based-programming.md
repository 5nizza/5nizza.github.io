---
title: "LCS's: breating life into message sequence charts"
tags: statecharts
layout: post
---

[paper](http://www.wisdom.weizmann.ac.il/~harel/SCANNED.PAPERS/LSCs.pdf)\
[bibtex]({{ site.url }}/assets/bibs/damm2001.bib)

Systems are composed of several processes that communicate synchronously or asynchronously.
These processes have internal variables.
There is no need for the exact description of the processes, only these interfaces.
Then, _live sequence charts_ allow one to specify the sequence of messages that
should or can happen during an execution
(also, there exists an execution such that eventually such events happen).
These scenarios can be guarded by conditions on the variables:
"if the condition on the variables hold, then the following sequence of events should/can happen".
It appears to me that this can be encoded using CTL\*.
So, in this sense, the contribution is more in the language itself:
how the sequences should look like, which kind of lines to use (dotted or solid or dashed etc.)
for drawing these diagrams.
(The synthesis from LCSs would be a kind of distributed synthesis from CTL\* specifications.)

