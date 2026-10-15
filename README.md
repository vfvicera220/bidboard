# BidBoard

Live auctions for small sellers: a replacement for Facebook comment bidding.

A 20-day full-stack learning project: Flutter, NestJS, gRPC, PostgreSQL, Python, Databricks,
FastAPI and AWS (ECS, IAM, CloudWatch).

- [docs/course-outline.md](docs/course-outline.md) - the original 20-day course outline and resources
- [docs/learning-plan.md](docs/learning-plan.md) - the day-by-day plan applying it to BidBoard

## Layout

| Path | What |
|---|---|
| `app/` | Flutter app (seller + buyer) |
| `proto/` | `bidboard.proto`, the shared gRPC contract |
| `services/auction/` | NestJS auction service |
| `services/notification/` | NestJS notification service |
| `services/estimate/` | FastAPI start-price and shill-bid service |
| `data/` | Synthetic data generator and Databricks notebooks/jobs |
| `db/migrations/` | Hand-written SQL migrations |
| `infra/` | Infrastructure as code for AWS |

## Local quick start

```sh
cp .env.example .env
docker compose up -d   # Postgres on localhost:5432
```

## Out of scope

Real payments, Facebook integration, iOS signing and App Store publishing.
