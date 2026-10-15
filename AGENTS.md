# AGENTS.md

Rules for any AI coding assistant working in this repository (Claude Code, Cursor, Copilot, Codex,
Gemini CLI). This file is the single source of truth; `CLAUDE.md` only imports it.

## Project

BidBoard is a live auction app and a learning project. Favour code that is clear and teaches the
concept over clever or heavily abstracted code. Explain unfamiliar framework idioms briefly when
introducing them.

## Stack

| Module | Stack |
|---|---|
| `app/` | Flutter, Dart, Riverpod |
| `services/auction`, `services/notification` | NestJS, TypeScript, Prisma, Jest |
| `services/estimate` | Python 3.12, FastAPI, Pydantic, pytest, uv |
| `data/` | Python, PySpark, Delta Lake (Databricks) |
| `proto/` | Protocol Buffers v3 |
| `db/migrations/` | Plain SQL |
| `infra/` | Infrastructure as code (AWS ECS Fargate, RDS, IAM, CloudWatch) |

## Rules

- `proto/bidboard.proto` is the contract. Change it first, then regenerate TS and Dart stubs. Never
  hand-edit generated stubs. Never reuse or renumber a protobuf field.
- Bid rules live in the auction service domain layer, never in controllers or the client:
  minimum increment, no bids after close, no self-bidding, anti-sniping extension.
- The bid table is an append-only ledger. Server timestamps decide ordering, never client time.
- Schema changes are new SQL files in `db/migrations/`. Never edit an applied migration.
- Every change ships with tests in that module's own runner (`flutter test`, `npm test`, `pytest`).
- Use synthetic data only. Never commit real personal data, secrets, keys or `.env`.
- AWS: no NAT Gateway, no Load Balancer, least-privilege IAM. Cost must stay near zero.
- TypeScript strict mode. Dart and Python with full type hints.
- Keep the diff small and focused. Do not add dependencies without saying why.

## Learning log

After a meaningful step, suggest a one-line entry for `LEARNING_LOG.md`: what worked, what was
confusing, what to revisit.
