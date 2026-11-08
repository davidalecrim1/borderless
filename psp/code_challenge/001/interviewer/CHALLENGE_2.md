# Challenge 02 — Rate Limiter Package (Interviewer)

**Time box:** 30 minutes

## Questions

- How many replicas run behind the load balancer? Ask once and let them react.
- Where does the counter live, and what makes the read-modify-write atomic?
- Which clock decides when the window rolls over?
- The store is down. Fail open or fail closed?
- Which JWT claim identifies the caller, and who verifies the signature?
- Do you rate limit before or after authentication? Why?
- What does a throttled client receive: status, headers, retry hint?
- What goes in the log line, and what must never appear in it?

## Hidden Requirements

- Chosen algorithm to handle the rate limiter.
- The rate limiter should allow `100 requests/minute` for normal users and `1000 requests/minute` for premium users.
- Deciding if the rate limiter will be for a single service (in memory) or across multiple services.
- Atomic increment, or the check-then-set race corrupts the counter.
- Limits are configuration, not constants inside the handler.
- No JWT or PII in logs.
- Missing, malformed, and expired tokens are handled.
- An explicit failure mode when the store is unavailable.
- 429 with `Retry-After` and the rate limit headers.
- Adoption is a middleware plus a config block, nothing more.
- AI may write the code. The candidate must be able to explain every line or it does not count.
