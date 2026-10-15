# BidBoard — 20-Day Learning Plan

Personal 20-day full-stack learning plan, built around one capstone: **BidBoard**, a live
auction app that replaces Facebook comment bidding ("mine/steal" threads) for small sellers.

## Ground rules

- **Budget:** ~$5 AWS, $0 everything else. Udemy optional (max 2-3 courses, on sale, for weak spots).
- **AI tools:** Claude Code (existing subscription) for most of the build; Cursor Free for the
  Cursor-specific exercises (Days 1, 18) and spot use. Repo is harness-agnostic (`AGENTS.md`).
- **Leverage what you know:** Spring DI → NestJS DI, React/Redux → Flutter state, React Native →
  Flutter mobile concepts, ValConnect Postgres/Flyway → schema and migration design. Compress
  those; spend the saved time on Dart, gRPC, PySpark and ECS.
- **Every day ends with a commit and a one-line entry in `LEARNING_LOG.md`**: what worked, what
  confused you, what to revisit.
- **Data:** synthetic only (generated bidders/listings). Never use work or customer data.
- **Time box:** ~6-7 focused hours/day. If a day overruns by >50%, cut the stretch goal, not the core.

## Target architecture

```
Flutter app (seller + buyer)
   │  gRPC (bidi stream: LiveAuction)
   ▼
auction-service (NestJS) ──gRPC──► notification-service (NestJS)
   │            │
   │            └──REST/gRPC──► estimate-service (FastAPI: start-price + shill-bid flag)
   ▼                                   ▲
Postgres (RDS) ──JDBC──► Databricks (PySpark, Delta) ──► features/aggregates
AWS: ECS Fargate · IAM · RDS · S3 (photos) · Secrets Manager · CloudWatch
```

## Repo layout

```
bidboard/
  AGENTS.md            # single source of AI rules
  CLAUDE.md            # contains: @AGENTS.md
  LEARNING_LOG.md
  proto/               # bidboard.proto — shared contract
  app/                 # Flutter
  services/auction/    # NestJS
  services/notification/
  services/estimate/   # FastAPI
  data/                # Databricks notebooks/jobs
  db/migrations/       # hand-written SQL migrations
  infra/               # IaC (CDK or Terraform) — one-command up/down
  docker-compose.yml   # local Postgres + services
```

## Plan at a glance

| Day | Focus | BidBoard milestone |
|---|---|---|
| 1 | Setup + Cursor | Repo, AGENTS.md, hello world in every layer |
| 2 | BMAD + TEA | BidBoard brief + first story; Playwright/API harness |
| 3 | Dart + Flutter basics | Listing browse screen (static) |
| 4 | Flutter state | Riverpod chosen; listing list/detail with state |
| 5 | Flutter shipped-app | Create-listing form + mock REST + tests + release build |
| 6 | NestJS DI | auction-service skeleton, modules/providers |
| 7 | NestJS real service | Auth, bid rules, Postgres, Jest e2e |
| 8 | Protobuf + gRPC | bidboard.proto, stubs for TS + Dart |
| 9 | gRPC in practice | LiveAuction bidi stream; Flutter on gRPC; notification-service |
| 10 | Python | Synthetic data generator |
| 11 | Databricks basics | Notebook reads Postgres bids |
| 12 | PySpark + Delta | Seller analytics + bidder features in Delta |
| 13 | FastAPI | estimate-service: start price + shill flag |
| 14 | Databricks jobs | Scheduled job feeding estimate-service |
| 15 | AWS | auction + estimate on ECS, RDS, CloudWatch |
| 16 | Postgres design | Ledger schema review, concurrency, EXPLAIN |
| 17 | Capstone 1 | End-to-end live bidding on two devices |
| 18 | Capstone 2 | ML + data wired in; Cursor agent-mode day |
| 19 | Capstone 3 | Full deploy, alarms, hardening |
| 20 | Buffer + demo + retro | Demo recording + written retro; tear down AWS |

---

## Day 1 — Setup, Cursor, big picture

**Learn:** docs.cursor.com (Rules, Tab, Agent pages). How the layers talk to each other (diagram above).
**Do:**
1. Install Flutter SDK + Android emulator, Node LTS + Nest CLI, Python 3.12 + uv, Docker, protoc,
   AWS CLI, Databricks CLI.
2. Create AWS account → **Budget alert at $1** before anything else. Enable MFA on root; create an
   admin IAM user/SSO for daily use.
3. Sign up for Databricks Free Edition.
4. Create the `bidboard` repo, `AGENTS.md`, `CLAUDE.md` (`@AGENTS.md`), `LEARNING_LOG.md`.
5. Hello world per layer: blank Flutter app, blank Nest app, Postgres via docker-compose, a Databricks
   notebook printing a Spark DataFrame, `aws sts get-caller-identity`.
6. Use **Cursor agent mode** (not a CLI generator) to scaffold the Nest app, and write one Cursor rule.

**Done when:** every layer runs locally; budget alert is confirmed by email.

## Day 2 — BMAD + TEA

**Learn:** docs.bmad-method.org ("first projects", "planning depth"); TEA overview.
**Do:**
1. Install BMAD for **both** Claude Code and Cursor.
2. Run the Clarify → Plan loop to produce a short BidBoard brief and the first story: "Seller creates
   a listing".
3. Build that story's skeleton in the Nest app with `bmad-build`; finish with a Learn-and-adjust note.
4. Install TEA; scaffold the Playwright/API test framework; run one `test-design` pass on the bidding
   rules (min increment, no bids after close, no self-bid, anti-sniping).

**Done when:** one story has gone round the full loop; TEA test-design doc exists in the repo.

## Day 3 — Dart + Flutter fundamentals

**Learn:** dart.dev/language (null safety, futures, async/await, mixins); Flutter widget tree,
Stateless vs Stateful, Row/Column/Stack, navigation. *Map from React Native: widgets ≈ components,
but layout is composition, not style props.*
**Do:** Build a static listing browse screen and a listing detail screen with navigation.
**Done when:** both screens render on the emulator from hard-coded data.

## Day 4 — Flutter state management

**Learn:** Provider vs Riverpod vs Bloc — read one example of each. *Riverpod ≈ hooks + context
done right; Bloc ≈ Redux with events.*
**Do:** Pick **Riverpod** (closest to your React mental model). Move listings into a provider; add a
fake countdown timer per listing.
**Done when:** a one-page note in LEARNING_LOG on why Riverpod was picked over the other two.

## Day 5 — Flutter to shipped-app quality

**Do (morning):** HTTP client against a mock REST API (json-server or a stub Nest route); JSON
(de)serialisation (`json_serializable`); the create-listing form with validation.
**Do (afternoon):** error/crash handling, widget + unit tests, a release-mode Android build, a quick
DevTools performance profile.
**Stretch (skip first if late):** release signing config.
**Done when:** `flutter test` passes; release APK installs on the emulator.

## Day 6 — NestJS fundamentals + DI

**Learn:** docs.nestjs.com Fundamentals (providers, custom providers, injection scopes, lifecycle).
*Mirrors Spring: module ≈ @Configuration, provider ≈ @Service/@Bean, scopes ≈ singleton/request/prototype.*
**Do:** auction-service with Listings and Bids modules, a custom provider (a `Clock` you can fake in
tests), and a custom `@CurrentUser()` decorator.
**Done when:** you can explain how Nest resolves a provider graph, and a unit test swaps the `Clock`.

## Day 7 — NestJS real service

**Learn:** DTOs + class-validator, guards/interceptors/pipes, exception filters, JWT, Prisma
(or TypeORM — pick one and note why).
**Do:** Postgres-backed listings and bids; JWT auth with seller/buyer roles; bid rules enforced in a
service with typed exceptions; Jest e2e tests. Structure it so the same service class can later be
exposed over gRPC.
**Done when:** e2e tests cover every bid rule from the Day 2 TEA test-design.

## Day 8 — Protocol Buffers + gRPC

**Learn:** protobuf.dev proto3 guide; the four call types; safe field evolution (never reuse field numbers).
**Do:** Write `proto/bidboard.proto`: `ListingService` (unary), `LiveAuction` (bidirectional:
`PlaceBid` up, `AuctionEvent` down), `NotificationService` (unary). Generate TS and Dart stubs from
the same file with a single script.
**Done when:** `make proto` (or an npm script) regenerates both stubs cleanly.

## Day 9 — gRPC in practice

**Learn:** docs.nestjs.com/microservices/grpc; gRPC status codes vs HTTP.
**Do:** expose auction-service over gRPC; build notification-service and call it from auction-service
(outbid, winner); switch the Flutter app from mock REST to gRPC with the live bid stream; map domain
errors to gRPC status codes.
**Done when:** two emulator instances (or emulator + desktop build) see each other's bids live.

## Day 10 — Python fundamentals

**Learn:** docs.python.org/3/tutorial — fast through basics; slow down on comprehensions, typing,
venvs/uv, dataclasses.
**Do:** Write `data/generate.py`: synthetic sellers, bidders (including a few shill accounts with
suspicious patterns), listings and bid histories; load them into Postgres.
**Done when:** ~10k bids are in Postgres with a reproducible seed.

## Day 11 — Databricks fundamentals

**Learn:** Databricks Free Edition training — workspace, notebooks, DataFrames.
**Do:** Read the bids/listings tables from Postgres over JDBC.
*If Free Edition can't reach your Postgres, export to CSV/Parquet and upload — note the limitation.*
**Done when:** a notebook shows bid counts per listing.

## Day 12 — PySpark + Delta Lake

**Do:** Delta tables for: final price vs start price by category; best hour-of-day to end an auction;
per-bidder features (bid velocity, % of bids on one seller, win rate, bids just before close).
Practise joins, window functions and aggregations.
**Done when:** the feature and analytics tables are saved as Delta with a short notebook write-up.

## Day 13 — FastAPI estimate-service

**Learn:** fastapi.tiangolo.com tutorial.
**Do:** `POST /start-price` (simple regression on category + history) and `POST /shill-score`
(rule-based or logistic regression on the bidder features). Pydantic models, tests, Dockerfile.
Call it from auction-service (auction-service flags suspicious bids; it does not block them).
**Done when:** the container runs and Nest calls it in a test.

## Day 14 — Databricks Jobs

**Do:** Turn the Day 12 notebook into a scheduled job; export features to Postgres (or a file the
estimate-service loads on start). Note the cost/cluster-size trade-offs and how Databricks on AWS
uses S3 and a VPC.
**Done when:** the job runs on a schedule and estimate-service reads fresh features.

## Day 15 — AWS: ECS, IAM, CloudWatch

**Learn:** AWS Skill Builder (ECS, IAM, CloudWatch digital courses).
**Do (IaC, so teardown is one command):** ECR repos; ECS Fargate cluster; auction-service +
estimate-service tasks (0.25 vCPU / 0.5 GB) in a **public subnet, no NAT, no ALB**; task execution
role + least-privilege task roles; Secrets Manager for DB creds; RDS Postgres micro; CloudWatch
log groups, a metric and one alarm.
**Done when:** both services are healthy on ECS; **scale to 0 at end of day**.

## Day 16 — Postgres schema + data-layer design

**Do:** Review the schema from scratch, not from the ORM: append-only `bid` ledger; server timestamps;
constraints (bid > current + increment enforced in a transaction); `SELECT … FOR UPDATE` on the
auction row vs `SERIALIZABLE` — test both with concurrent bids. Indexes: composite
`(auction_id, created_at desc)`, partial index on open auctions. EXPLAIN ANALYZE before/after on 2-3
hot queries. Write it up as migrations.
**Done when:** a concurrency test firing 50 simultaneous bids yields exactly one valid winner per price step.

## Day 17 — Capstone 1: core slice

**Do:** Flutter → gRPC → auction-service → Postgres, end to end, including anti-sniping (a bid in the
last 30s extends the close by 30s) and the winner shown as "awaiting payment".
**Done when:** live bidding works reliably on two devices locally. Don't start Day 18 work until it does.

## Day 18 — Capstone 2: data + ML (Cursor day)

**Do:** Start-price suggestion in the create-listing form; shill-flag badge for sellers; the Databricks
job feeds the features. Use **Cursor agent mode deliberately** for this day's work and log what it was
good and bad at compared with Claude Code.
**Done when:** a new listing gets a suggested price, and a seeded shill bidder gets flagged.

## Day 19 — Capstone 3: deploy + harden

**Do:** Deploy the final images to ECS; wire CloudWatch alarms (error rate, bid latency); run the TEA
`nfr-assess` and `trace` workflows; fix the top issues. Catch up here if Day 17 or 18 overran.
**Done when:** the deployed stack passes the TEA suite.

## Day 20 — Buffer, demo, retro

**Do:** close loose ends; record a 3-5 minute demo (two phones bidding, anti-snipe, shill flag,
CloudWatch view); write the retro (what was intuitive, what needs another day, Riverpod/Prisma
verdicts, Cursor vs Claude Code). **Tear down AWS and confirm $0 running cost.**
**Done when:** demo video + retro are in the repo; AWS console shows no running resources.

---

## Cost guardrails (check daily)

- [ ] Budget alert active at $1
- [ ] ECS services scaled to 0 when not in use
- [ ] No NAT Gateway, no Load Balancer
- [ ] RDS stopped when idle (auto-restarts after 7 days — re-stop it)
- [ ] Full teardown on Day 20

## Out of scope

Real payments, Facebook integration, iOS signing/App Store, production security review.

## Free core resources

docs.cursor.com · docs.bmad-method.org · TEA docs · docs.flutter.dev · dart.dev/language ·
docs.nestjs.com · protobuf.dev · grpc.io · docs.python.org/3/tutorial · Databricks Free Edition +
training · fastapi.tiangolo.com · AWS Skill Builder · postgresql.org/docs
