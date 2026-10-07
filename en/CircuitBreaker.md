# CIRCUIT BREAKER

What is Circuit Breaker?

In distributed systems, failures are not an exception, they are inevitable. When an external API slows down, a database becomes unavailable, or a service starts returning errors, the biggest risk is not only the initial failure. The real problem happens when that failure propagates and starts consuming threads, connections, and resources until it brings down healthy parts of the system. That is exactly why the Circuit Breaker pattern exists.

Circuit Breaker is a resilience pattern that interrupts calls to a service that is continuously failing.

The idea is simple: instead of continuing to access an unavailable resource and making the problem worse, the system detects failures and “opens the circuit”, temporarily stopping new requests. It follows the same concept as the electrical circuit breaker in your house.

Circuit Breaker States

Closed: Everything works normally. Requests continue flowing to the service.
If the error rate exceeds the configured threshold, it changes to Open.

Open: The system stops sending requests and immediately returns an error or executes a fallback.
Application → Open Circuit → Immediate Response
Goal: reduce pressure on the failing service, avoid cascading timeouts, and free internal resources.

Half-Open: After a waiting period, some requests are allowed through.
If they succeed, the circuit closes again.
If they fail, it opens again.