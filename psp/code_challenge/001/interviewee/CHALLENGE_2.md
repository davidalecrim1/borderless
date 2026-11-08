# Challenge 02 — Rate Limiter Package

**Time box:** 30 minutes
**Mode:** AI-assisted. The design is yours; the AI writes the code.

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

- Share your thought process as you write the code and feel free to ask questions.
- Work with an AI coding assistant of your choice: Claude Code, Codex, OpenCode, Cursor, Copilot,
  or whatever you already use day to day.
- Think about the requirements and make sure you understand the problem and the solution.

## Deliverables

1. The package: public API, configuration, and the limiting logic.
2. The `GetTrades` service wiring the package.
