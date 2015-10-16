# HOW TO HAVE YOUR CAUSALITY AND WALL CLOCKS, TOO

Jon Moore

http://thestrangeloop.com/2015/how-to-have-your-causality-and-wall-clocks-too.html

Works at comcast, relatively common architecture
Request comes in, and utilizes both Server A and B
But if the clocks are not well synchornized, the log line from B and A
may be reversed in the log.

The timestamps don't reflect the causal relationships.

Lampert figured out causal relationships between sending messages
between processes, and that we could assign "timestamps" --
monotonically increasing numbers --  accordingly. Logical clocks.

But there's a problem: we have to do production support. Refering to
"timestamp 42" isn't very helpful when you're paged in the middle of
the night.

SUNY Buffalo "Hybrid logical clocks" -- "3:11pm|7" to combine wall
clock + counter (Lampert clock). Most significant bits are the real
clock. When we're assigning a timestamp, we look at the last timestamp
we received and the system clock. If the system clock is ahead of the
previous physical time, we use the system clock and reset the counter
to 0. But if it's behind, we retain the previous messages wall-clock
time component and increment the counter.


NB: Is this like the request ID that we pass through requests? Or
within a service? Or both?


"This is really cool."

It seems like what we have here is the best of both worlds. We have
something that makes sense to humans, and it also correctly reflects
causality, and we can order our messages in a logical fashion.

But what if NTP isn't keeping the clocks sufficiently in sync? Or if
it gives up on sync? Or stops running? Or if someone screws up the
time on one of the servers?

For example, what if you fat-fingered a time update, and set it to
2051 instead of 2015? That one would drag everyone else into the
future. And you couldn't fix it until 2051!

[This guy does a good job attributing CC licensed photos in slides.]

**Distributed Monotonic Clocks** are built on these hybrid clocks

First, we need a reset button. By adding an epoch as the leading
most-significant bit, we can deal with the "2051" problem. A Reset
operation takes the local time, and increments the epoch. So even if
the local time goes "backward", the epoch increment means we still
reflect causality.

But it's still sort of a bummer that we have to notice the problem and
issue the reset operation. One thing we can do is set a limit on how
far we'll jump local time into the future. "Hrmmm, I'm 5 hours ahead
of the last one, maybe I'll just increment the counter instead of the
time."

But how far ahead of _what_? Sort of implies coordination. It'd be
nice to piggyback on our normal protocol; ie, as you receive a
message, tell your clock to "deal with it" [it being the previous
timestamp embedded in the payload].

But who should we listen to? When we come up we don't really know
about who our neighbors are, who we should pay attention to.

Population Protocols describe how to neighbors can communicate, and
how they'll converge on some state.

[Describes how a hierarchy of state sharing groups can self organize.]

Convergence time is roughly O(log n), which isn't super surprising
given that it's roughly a tree-like structure. Similarly, the number
of levels in the hierarchy is also roughly O(log n).

-----

Once we've self-organized into a hierarchy, _now_ we can reliably
detect how far we've drifted from the the median, and when we're too
far off, run in this degraded mode.


Open Issues:

* Payload size: this is a piggy-back protocol, so when we start
  getting into systems with thousands of nodes, the amount of metadata
  could get biggish. One possibility would be to stop sharing metadata
  about lower levels once the groups stabilize.

* How do nodes join after the groups have self-organized?

* How do nodes leave, and how do we recover?


Q & A

Talked about reset button about handling clocks too far ahead, what
about too far behind?

Not as problematic; you can just update the clock, and being behind
won't drag others along. But they could also detect how far behind the
median they are, and raise their hand for help.
