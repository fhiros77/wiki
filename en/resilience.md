# RESILIENCE IN SYSTEMS

Resilience in systems is not about preventing failures, it’s about surviving them.

For a long time, I associated technical quality with the absence of failures. If an application went down, the conclusion seemed obvious: the architecture was wrong.

Over time working with distributed systems, I realized something different: failures are not exceptions, they are part of the normal behavior of the environment. Networks fluctuate, dependencies become slow, services restart, connections expire, memory usage grows, and consumers stop consuming.

The question stopped being, “How do we prevent failures?” and became, “How does the system keep running when they happen?”

When retry becomes part of the problem, one of the first traps I encountered was believing that retry solved unavailability. In practice, uncontrolled retries can create the exact opposite effect. Imagine an already overloaded service receiving hundreds of simultaneous retry attempts. The initial incident becomes a bigger incident. Today, before thinking about retry, I usually think about: explicit timeouts, retry limits, exponential backoff, idempotent operations, and protection against cascading failures.

The goal stopped being to insist harder and became failing in a controlled manner, meaning automatic recovery stopped being optional.

In asynchronous integrations, losing connection should not require human intervention. If a consumer disconnects and only comes back when someone notices, the system is not truly resilient. What became part of the design: automatic disconnection detection, safe reprocessing, controlled re-subscription, prevention of multiple concurrent reconnections and recovery cycle observability.

Resilience does not happen when everything works, but when the system can recover by itself. Not every failure needs to bring everything down, we should continue delivering part of the value because partial unavailability is better than total unavailability. I realized that availability does not mean delivering 100% all the time. It means preserving what is essential. That may involve responding with cached data, decoupling processing, temporarily reducing functionality, or prioritizing critical operations.

A system can appear healthy while still degrading: maintaining orphan connections, increasing memory consumption, resources that are never released, buffers accumulating. These problems rarely appear as explicit errors, they usually show up as strange behavior before the incident.

Today, when I think about resilience, I usually think about four questions:
What happens when a dependency fails?
How does the system recover on its own?
Does the error propagate or stay isolated?
How will I discover this before the user does?

Conclusion:
Robust systems are not the ones that never break, but the ones that remain useful when something breaks.