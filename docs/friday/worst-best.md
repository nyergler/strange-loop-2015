When Worst is Best
(in distributed system design)

What would happen if we designed systems to handle the absolute worst
case scenario? You _could_ provision for 7.3B users, but you'd wind up
with a bunch of idle resources most of the time. And what about
hardware, like if you wanted to put your laptop on the Mars
rover. It's expensive. Or in terms of security: what if your
developers are all malicious and against you? Prod

Designing for the worst case winds up penalizes the average case.

Distributed systems matter: almost every non-trivial application today
is, or is becoming, distributed. The defining characteristic of
distribution is that it happens over a network. Therefore, almost
every non-trivial application today needs to worry about the network.

Networks are Hard.

So many things can go wrong.

* Packets can be delayed
* Packets can be dropped

Availability is a strategy for addressing worst-case network
behavior. Typically defined as any replica being able to respond to
any request. And if we have this property, it implies indendence and
no coordination.

This has nice implications for average case.

* "Infinite" scale out: just spin up more instances of the
  application.

* Coordination free improves throughput, and even helps in single
  server situations, since it implies thread/core independence.

* It also ensures lower latency, since there's a lower bound on how
  fast we can communicate across the network.

* And in the case of failure, it ensures "always on" response

But what about CAP!?!

Turns out many single node databases use coordination internally. But
what if we think about designing a database without coordination? Is
it strictly necessary? Turns out it isn't, and there are some easy
wins.

Punchline: systems that behave well during network fault behave better
in non-faulty environments, too.

[He keeps saying "in the real world" without talking about what that
world is.]
