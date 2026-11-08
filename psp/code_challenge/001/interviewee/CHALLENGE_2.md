# Challenge 02 — Rate Limiter Package

**Time box:** 45 minutes
**Mode:** design first, then implementation. AI may write the code; the design is yours.

## Context

You are joining a company with dozens of Go microservices. Every service exposes HTTP APIs and
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
