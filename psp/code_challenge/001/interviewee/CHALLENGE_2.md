# Challenge 02 — Rate Limiter Package

**Time box:** 30 minutes
**Mode:** AI-assisted. The design is yours; the AI writes the code.
**Language:** any.

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

## Rules

- Ask questions before writing anything. Questions are encouraged, including ones that challenge
  the requirements or the interviewer's assumptions.
- Work with an AI coding assistant of your choice: Claude Code, Codex, OpenCode, Cursor, Copilot,
  or whatever you already use day to day.
- You own the design. The assistant writes the code you ask for; you decide what it should do and
  why.
- Read what it produces. You are responsible for every line that lands in the file, and if you
  cannot explain a line, it does not count.

## Deliverables

1. The package: public API, configuration, and the limiting logic.
2. The `GetTrades` service wiring the package.
3. A short note on the trade-offs you made.
