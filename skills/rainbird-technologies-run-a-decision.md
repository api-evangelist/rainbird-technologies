---
name: Run a Rainbird decision
description: Query a published Rainbird Knowledge Map by starting a session, injecting known facts, running a query, and answering the questions Rainbird asks until it returns a decision.
api: openapi/rainbird-technologies-openapi-original.json
operations: [start, inject, query, response]
---

# Run a Rainbird decision

Use this skill to get an explainable decision from a published Rainbird Knowledge Map (KM).

## Auth
Send your API key in the `X-API-Key` header on every request (legacy HTTP BasicAuth is still accepted but deprecated). Base URL is `https://api.rainbird.ai` (Community) or `https://enterprise-api.rainbird.ai` (Enterprise).

## Steps
1. **Start a session** — `start`: `GET /start/{kmID}` with your `kmID`. Capture the returned `sessionID`. Sessions expire 24h after last update.
2. **Inject known facts** (optional) — `inject`: `POST /{sessionID}/inject` with the facts you already know (subject/relationship/object triples) so Rainbird asks fewer questions.
3. **Run the query** — `query`: `POST /{sessionID}/query` with the concept/relationship/subject you want a decision on. The response is either a `Question` (more input needed) or a `ResultResponse` (a conclusion).
4. **Answer questions** — while a `Question` comes back, `response`: `POST /{sessionID}/response` with the answer, and re-check for a result. Repeat until a `ResultResponse` is returned.

## Rules
- Errors are plain HTTP codes: 400 (missing parameter), 401 (invalid key), 403 (key lacks access), 404 (session/KM not found), 50x (server-side). See `errors/rainbird-technologies-problem-types.yml`.
- There is no idempotency key and no pagination; the API is stateful per session (`conventions/rainbird-technologies-conventions.yml`).
