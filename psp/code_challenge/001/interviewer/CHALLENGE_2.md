# Challenge 02 — Rate Limiter Package (Interviewer)

**Time box:** 30 minutes

## Questions

1. What is the limit: per second, per minute, per user, per tenant? Is there a burst allowance?
2. How many replicas run behind the load balancer? Ask once and let them react.
3. Where does the counter live, and what makes the read-modify-write atomic?
4. Which clock decides when the window rolls over?
5. The store is down. Fail open or fail closed?
6. Which JWT claim identifies the caller, and who verifies the signature?
7. Do you rate limit before or after authentication? Why?
8. What does a throttled client receive: status, headers, retry hint?
9. What goes in the log line, and what must never appear in it?
10. Pick a function the AI wrote and walk me through it.
11. What did you change after the first generation, and how do you know it is correct?
12. What does a new team have to write to adopt the package?

## Hidden Requirements

- Multi-replica correctness. An in-process map is a per-pod limit, not a service limit.
- The rate limit key comes from a verified claim, never from a client-controlled header.
- Atomic increment, or the check-then-set race corrupts the counter.
- Limits are configuration, not constants inside the handler.
- No JWT or PII in logs.
- Missing, malformed, and expired tokens are handled.
- An explicit failure mode when the store is unavailable.
- 429 with `Retry-After` and the rate limit headers.
- Adoption is a middleware plus a config block, nothing more.
- AI may write the code. The candidate must be able to explain every line or it does not count.
