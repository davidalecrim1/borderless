# Challenge 02 — Rate Limiter Package (Interviewer)

**Time box:** 30 minutes
**Mode:** AI-assisted. The design is the candidate's; the AI writes the code.
**Language:** any. The choice is not scored.

## Context

You are joining a company with dozens of microservices. Every service exposes HTTP APIs and
authenticates requests with a JWT. Product wants one rate limiting package that every team can
import, instead of each service rolling its own.

## Task

Design and implement a reusable rate limiter package, plus one example service that uses it.

### The package

It will be consumed by other teams, so the public API matters as much as the implementation.

### The example service

`GetTrades` is a read-only endpoint. The request carries a JWT. For each request the service must:

- identify the caller from the JWT;
- log the HTTP request;
- apply the rate limit for that caller.

## Deliverables

1. The package: public API, configuration, and the limiting logic.
2. The `GetTrades` service wiring the package.
3. A short note on the trade-offs you made.

---

## Interviewer notes

### Shape of the session

Minutes 0-8: requirements Q&A. Minutes 8-12: design on the whiteboard, no code. Minutes 12-27:
implementation with the AI. Minutes 27-30: review and trade-offs.

Open by saying questions are encouraged. The requirements block is where this challenge is won, and
a candidate who treats it as a formality has already given away the strongest signal.

If the candidate opens an editor before minute 12, interrupt and ask for the design first. The
challenge is a design challenge disguised as a coding challenge.

### Questions the candidate should ask you

Answer only what is asked. If they ask none of these, that is the headline of your feedback.

- What is the limit: requests per second, per minute, per user? Is there a burst allowance?
- Is the limit global, per tenant, per endpoint or per plan tier?
- How many replicas does the service run behind the load balancer?
- What happens when the limit store is down: fail open or fail closed?
- Which claim in the JWT identifies the caller? Is the JWT verified, and by whom?
- What must the log line contain, and what must never appear in it?
- What response does a throttled client get?

### The requirement that decides the design

The candidate will probably start with an in-memory `map[string]*bucket` behind a mutex. It is
correct for one process. Ask, once and only once:

> "How many replicas does `GetTrades` run?"

Expect them to rework the design around a shared store (Redis) or to defend a per-replica limit
with a documented approximation. Both answers are acceptable if the trade-off is explicit. Silence
after the question is a fail.

### Gotchas to probe

1. **Auth order.** Rate limiting before JWT validation means you can only key by IP; after it means
   you pay for validation on every abusive request. Best answers split the two: a cheap IP-level
   limit in front, the per-user limit after authentication.
2. **The read-only detail.** Read-only does not mean free or idempotent from a cost perspective.
   Ask whether a retried request consumes quota, and whether `GetTrades` bursts come from
   pagination loops.
3. **Window semantics.** Fixed window allows 2x the limit across a boundary. Sliding log is exact
   and expensive. Sliding window counter is the usual compromise. Token bucket allows bursts.
   Ask why they chose theirs and how much state per user it costs.
4. **Atomicity.** `GET` then `SET` on Redis races. Expect `INCR` plus `EXPIRE`, or a Lua script, or
   a library with a documented atomic path. Ask what happens when the `EXPIRE` is lost.
5. **Clock.** Any window based on the service clock drifts across replicas. Does the design use the
   store's clock or the pod's?
6. **Store failure.** Redis down: fail open (overload) or fail closed (outage). Either is fine;
   an unexamined default is not.
7. **JWT handling.** They must not trust `sub` before verifying the signature, must not log the raw
   token, must handle expired and missing tokens, and must decide whether an invalid token is
   counted against the IP limit or rejected outright.
8. **HTTP contract.** `429` with `Retry-After` and, ideally, `X-RateLimit-Limit`,
   `X-RateLimit-Remaining`, `X-RateLimit-Reset`. Ask if the headers are part of the package or the
   handler.
9. **Logging.** Structured, one line per request, with method, path, status, latency, user id and
   request id. No token, no PII. Ask whether the log happens before or after the limiter decision,
   and how they would correlate a throttled request with its client.
10. **Package ergonomics.** Interface versus concrete type, a config struct with sane defaults,
    injectable clock and store for tests, `context.Context` everywhere, per-route overrides,
    metrics hooks. Ask what a new team has to write to adopt it: if the answer is more than a
    middleware and a config block, the API failed.

### AI usage

AI may write the whole implementation. That is allowed. What is not allowed is the candidate being
unable to explain it.

Let them use their own tool — Claude Code, Codex, OpenCode, Cursor, Copilot, whatever they work
with. The choice of tool is not scored; how they drive it is.

- Pick one function the AI produced at random and ask them to walk through it line by line.
- Ask what they changed after the first generation and why.
- Ask how they know it is correct, and what test they would write first.
- A candidate who pastes a prompt, accepts the output, and moves on without reading it fails the
  challenge regardless of whether the code compiles.

### Red flags

- No requirements questions at all.
- In-memory limiter with no discussion of replicas.
- Rate limit keyed on something the client controls (a header, a query param).
- Raw JWT in logs.
- Check-then-increment with no discussion of races.
- Hard-coded limits inside the handler instead of configuration.
- No failure mode for the store.

### Scoring

| Dimension           | 1 (weak)                     | 3 (solid)                          | 5 (strong)                                          |
| ------------------- | ---------------------------- | ---------------------------------- | --------------------------------------------------- |
| Requirements        | assumes the rules            | asks about limits and identity     | extracts the failure modes and the multi-replica constraint |
| Package design      | single handler, no package   | clean API, config, middleware      | API a stranger can adopt in one file; testable seams |
| Distributed systems | unaware of replicas          | picks Redis, mentions atomicity    | names the races, the clock, the fail-open trade-off  |
| Security            | trusts the JWT blindly       | validates before use, no token in logs | separates unauthenticated and authenticated limits  |
| AI supervision      | accepts output unread        | reads and fixes output             | directs the AI, reviews the diff, proves behaviour  |
