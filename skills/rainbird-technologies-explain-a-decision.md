---
name: Explain a Rainbird decision
description: Retrieve the evidence chain, interaction log, and session detail behind a Rainbird decision to audit why the engine reached its conclusion.
api: openapi/rainbird-technologies-openapi-original.json
operations: [evidence, interactions, session]
---

# Explain a Rainbird decision

Use this skill to audit an existing Rainbird decision. Rainbird is deterministic and fully auditable, so every fact has a traceable evidence chain.

## Auth
Send `X-API-Key`. Evidence and interaction data are additionally protected: pass `x-evidence-key` for evidence and `x-interaction-key` for interaction logs (unless link-sharing has been enabled on the KM). Base URL `https://api.rainbird.ai` or `https://enterprise-api.rainbird.ai`.

## Steps
1. **Get session detail** — `session`: `GET /analysis/session/{sessionID}` to confirm the session and its state.
2. **Get the evidence chain for a fact** — `evidence`: `GET /analysis/evidence/{factID}/{sessionID}` with the `factID` from the decision result to see the subject/relationship/object triples and certainties that produced it.
3. **Get the interaction log** — `interactions`: `GET /analysis/interactions/{sessionID}` to see the full ordered log of start/query/inject/answers/results events.

## Rules
- Missing evidence/interaction keys return 403; a stale (>24h) session returns 404.
- Errors are plain HTTP status codes (`errors/rainbird-technologies-problem-types.yml`).
