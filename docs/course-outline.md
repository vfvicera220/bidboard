# 20-Day Tech Stack Crash Course

## **Program at a glance**

| Day(s) | Focus | Output |
| :---- | :---- | :---- |
| 1 | Cursor + architecture overview + environment setup | All tooling installed; "hello world" in every layer of the stack |
| 2 | BMAD Method & BMAD TEA (agentic delivery + testing) | A scoped change taken through BMAD's delivery loop, plus a Playwright harness scaffolded with TEA |
| 3-5 | Flutter (Provider/Riverpod/Bloc, shipped-app rigor) | A release-quality Flutter app calling a mock REST API |
| 6-7 | NestJS (DI/decorators depth, not just Express) | Auth'd, tested REST API backed by Postgres |
| 8-9 | gRPC & Protocol Buffers | NestJS gRPC service, incl. a service-to-service call, consumed by the Flutter app |
| 10-14 | Python fundamentals + Databricks (PySpark) + FastAPI for AI/ML services | Notebook/job reading Postgres data + a FastAPI model-serving microservice NestJS calls |
| 15 | AWS: ECS, IAM, CloudWatch | NestJS + Python service deployed on ECS, wired to Postgres and Databricks |
| 16 | PostgreSQL schema & data-layer design | A reviewed, indexed schema built for the capstone domain |
| 17-20 | Capstone build + demo (incl. a buffer/review day) | End-to-end feature: Flutter → gRPC → NestJS → Postgres/Python service → Databricks, deployed on ECS |

 

## **Day 1 — Tooling, Cursor, and the big picture**

Topics: install and configure Flutter SDK, Node/Nest CLI, Docker, Postgres, the Databricks CLI, and AWS CLI; walk through how the seven pieces of the stack talk to each other (mobile client → gRPC → backend services → Postgres for OLTP → Databricks for analytics → AWS for hosting); spend the bulk of the day going deep on Cursor itself, since it's the tool the team will use for the other 19 days — custom instructions/rules files, Tab/Composer/Agent modes, codebase-aware chat, and how to prompt it well for an unfamiliar framework.

Hands-on: get a "hello world" running in each layer (a blank Flutter app, a blank NestJS app, a local Postgres container, a Databricks free-edition workspace, and an AWS CLI login) and use Cursor's agent mode to scaffold at least one of them instead of a CLI generator.

Resources:

•       [Cursor Course: FullStack Development with Cursor (Udemy)](https://www.udemy.com/course/cursor-ai-ide/) — practical, workflow-focused

•       Official Cursor docs: <https://docs.cursor.com> — read "Rules," "Tab," and "Agent" pages specifically

•       [Cursor AI: The Only IDE You Need in 2026 (YouTube)](https://www.youtube.com/watch?v=ed26g7jxAbU) — fast visual overview if the team prefers video first

## **Day 2 — BMAD Method & BMAD TEA (Test Architect) module**

Topics: what BMAD-METHOD is — an agile, AI-driven delivery methodology built to keep decisions explicit and preserve context across a delivery loop of four phases: Clarify (turn a vague idea into a scoped request), Plan (produce a plan sized to the change), Build and verify (implement in small, reviewable increments), and Learn and adjust (feed outcomes back into planning). Cover how it "right-sizes" planning — go straight to implementation for a clear, small change, add deeper planning for a larger initiative — and how it plugs into Cursor through slash-command workflows rather than replacing the IDE. Then a lighter pass on BMAD TEA, the specialized quality/test-automation agent that extends BMAD across the delivery lifecycle: what problem it solves ("AI-generated tests that rot in review"), and an overview of its core workflows — test-design, framework, ci, atdd, automate, evaluate, test-review, nfr-assess, and trace — without going deep on all nine in one day.

Hands-on: install BMAD via the Skills CLI (npx skills add bmad-code-org/BMAD-METHOD) or the Cursor/Claude Code plugin route, run bmad setup and bmad-build against a small scoped change in one of the Day 1 "hello world" projects, and walk the full Clarify → Plan → Build and verify → Learn and adjust loop end-to-end at least once. Then install TEA alongside core BMAD and scaffold a Playwright test harness with the framework workflow against the same project, running one test-design pass — the deeper per-workflow practice (atdd, automate, test-review, etc.) happens organically during the capstone rather than as a dedicated exercise here.

Resources:

•       [Official BMAD-METHOD docs](https://docs.bmad-method.org) — start with "first projects" and "planning depth"

•       [GitHub — bmad-code-org/BMAD-METHOD](https://github.com/bmad-code-org/BMAD-METHOD) — README, install instructions, Discord/YouTube links

•       [Test Architect (TEA) docs](https://bmad-code-org.github.io/bmad-method-test-architecture-enterprise/) — start with "TEA Overview"

•       [GitHub — bmad-method-test-architecture-enterprise](https://github.com/bmad-code-org/bmad-method-test-architecture-enterprise)

## **Days 3-5 — Flutter**

**Day 3 (fundamentals I):** Dart language essentials for devs coming from TS/JS/Java (null safety, futures, async/await, mixins), the widget tree, StatelessWidget vs StatefulWidget, layout widgets (Row/Column/Stack), and navigation.

**Day 4 (state management survey):** compare the three state-management approaches the team should be conversant in — Provider, Riverpod, and Bloc — their tradeoffs and where each fits, then pick one to go deep on for the rest of the build (Riverpod or Bloc are the closest fit for devs coming from React/Angular-style state).

**Day 5 (hands-on + shipped-app quality + mini project):** a fuller day combining reps with production polish. Morning: build with the state-management library picked on Day 4 — no new concepts, just reps: calling REST/HTTP APIs, JSON (de)serialization, and forms and validation, all wired through that state layer. Afternoon: what separates a demo from a shippable app — release build configuration and signing (Android/iOS, at a conceptual level), crash and error handling, basic performance profiling, and widget/unit testing; then build a small Flutter screen that lists and creates records against a mock REST API, held to that shipped-app bar — this becomes the client shell the gRPC work plugs into on Day 9\. This day was compressed from two days into one to bring the course back to 20 days total; if it runs long, the shipped-app polish (release signing, profiling) is the part most safely pushed into spare time later rather than skipped.

Resources:

•       [Flutter & Dart – The Complete Guide (Udemy, Maximilian Schwarzmüller)](https://www.udemy.com/course/learn-flutter-dart-to-build-ios-android-apps/) — best pick for experienced devs; moves fast, explains *why* not just *how*

•       [The Complete Flutter Development Bootcamp with Dart (Udemy, Angela Yu / App Brewery)](https://www.udemy.com/course/flutter-bootcamp-with-dart/) — good alternative if the team prefers a project-first style

•       Official docs: <https://docs.flutter.dev> and the Dart language tour at <https://dart.dev/language> — use these as the reference once past day 3, they're excellent and current

## **Days 6-7 — NestJS**

**Day 6 (fundamentals + DI deep dive):** modules, controllers, providers, and dependency injection — go beyond "NestJS is Express with decorators": how the DI container resolves providers, scopes (singleton/request/transient), custom providers and custom decorators, and the request lifecycle. This is the concept senior devs from other typed ecosystems (Spring, .NET, Angular) tend to pick up fastest since it mirrors patterns they already know.

**Day 7 (building a real service):** DTOs and validation (class-validator), guards/interceptors/pipes, exception filters, connecting to Postgres via TypeORM or Prisma (confirm which with the client), JWT authentication, and unit/e2e testing with Jest — by end of day the service should be auth'd, validated, and tested, and structured so it's ready to expose both REST and gRPC (needed for Day 8).

Resources:

•       [NestJS: The Complete Developer's Guide (Udemy, Stephen Grider)](https://www.udemy.com/course/nestjs-the-complete-developers-guide/) — the strongest pick for senior devs; goes under the hood of the DI container instead of treating it as magic

•       [NestJS Ultimate Masterclass: Basics to Advanced (Udemy)](https://www.udemy.com/course/nestjs-ultimate-masterclass-basics-to-advanced-2026/) — solid alternative, updated for 2026

•       Official docs: <https://docs.nestjs.com> — the "Fundamentals" and "Techniques" sections are dense but excellent; keep this open both days

## **Days 8-9 — gRPC & Protocol Buffers**

**Day 8:** protobuf schema design (messages, services, versioning fields safely), gRPC's four call types (unary, server-streaming, client-streaming, bidirectional), and generating TypeScript stubs (for NestJS) and Dart stubs (for Flutter) from the same .proto file.

**Day 9:** implement a real gRPC service in NestJS using its built-in microservices transport, swap the Flutter app's mock REST calls (from Day 5\) for gRPC calls, and add one service-to-service call — a second small NestJS service that the first calls over gRPC — since service-to-service communication, not just client-to-backend, is explicitly part of the bar here. Cover interceptors, error handling, and status codes, since gRPC error semantics differ from HTTP status codes.

Resources:

•       [gRPC [Node.js] MasterClass: Build Modern API & Microservices (Udemy)](https://www.udemy.com/course/grpc-nodejs/) — closest match to the NestJS/TypeScript side of this stack

•       NestJS official gRPC guide: <https://docs.nestjs.com/microservices/grpc> — go here first, it's short and exactly matches what Day 9 needs

•       Official gRPC docs: <https://grpc.io/docs/languages/node/> and <https://grpc.io/docs/languages/> (there's no first-party Dart gRPC guide as polished as the others, so pair grpc.io's Dart quickstart with the grpc pub.dev package docs for the Flutter client side)

•       Protocol Buffers language guide: <https://protobuf.dev/programming-guides/proto3/>

## **Days 10-14 — Python fundamentals, Databricks & AI/ML services**

**Day 10 (Python fundamentals):** a full day of Python for devs coming from TypeScript/Dart/Java — syntax, data structures, virtual environments and pip, list/dict comprehensions, typing hints, and a short exercise or two, so Days 11-13 are about Databricks and FastAPI rather than fighting Python syntax.

**Day 11 (Databricks fundamentals):** Databricks workspace and notebooks, Spark DataFrame basics using PySpark, and reading data out of Postgres into a notebook (JDBC connector). With Python out of the way on Day 10, this day is fully hands-on with the Databricks/Spark paradigm itself.

**Day 12 (Databricks deep dive):** Delta Lake tables, and a full day of PySpark practice — transformations, joins, aggregations — against a dataset close to the capstone domain. This is the day to get comfortable with the paradigm shift from row-oriented app code to distributed DataFrames, at a pace that doesn't assume prior Spark exposure.

**Day 13 (Python for AI/ML service work):** build a small FastAPI microservice that serves a model or prediction relevant to the capstone domain, containerize it, and call it from NestJS (REST or gRPC, for consistency with Days 8-9).

**Day 14:** Databricks Jobs/Workflows for scheduling, wiring the Databricks output into the Python service or straight into Postgres, a light touch on cluster sizing/cost awareness, and how Databricks fits alongside AWS (which usually hosts the underlying storage/compute).

Resources:

•       [The Complete Python Bootcamp From Zero to Hero (Udemy)](https://www.udemy.com/course/complete-python-bootcamp/) — for Day 10; move quickly through the basics and slow down on the parts unfamiliar coming from TS/Dart/Java

•       Official Python tutorial: <https://docs.python.org/3/tutorial/> — good as a fast, authoritative reference alongside the course

•       Databricks Free Edition + self-paced training catalog: <https://www.databricks.com/learn/training/home> and <https://www.databricks.com/learn/free-edition> — free, official, and the fastest way to get real hands-on notebook (PySpark) time

•       [Databricks Certified Data Engineer Associate – Bootcamp (Udemy)](https://www.udemy.com/course/databricks-certified-data-engineer-associate-bootcamp/) — good structured path even if certification itself isn't the goal

•       Official docs: <https://docs.databricks.com> — "Get started" and "Delta Lake" sections cover Days 11-12 directly

•       [FastAPI – The Complete Course 2026 (Udemy)](https://www.udemy.com/course/fastapi-the-complete-course/) — for building the Day 13 model-serving service

•       [Deploy ML Model in Production with FastAPI and Docker (Udemy)](https://www.udemy.com/course/deploy-ml-model-in-production-with-fastapi-and-docker/) — hands-on model serving + containerization, directly matches Day 13

•       Official FastAPI docs: <https://fastapi.tiangolo.com>

## **Day 15 — AWS: ECS, IAM & CloudWatch**

The client has named the specific AWS surface to nail down: ECS (not EKS), IAM, and CloudWatch — hands-on with these, not just Azure-equivalent familiarity. Deploy the NestJS service and the new Python/FastAPI model-serving service as containers on ECS (Fargate); write IAM roles and policies for the ECS task execution role and least-privilege access to RDS/S3/Secrets Manager; wire up CloudWatch for logs, metrics, and a basic alarm; connect RDS for Postgres (parameter groups, backups, connecting securely from ECS); and confirm how a Databricks workspace on AWS reads from/writes to the same account's storage (S3) and networking (VPC).

Resources:

•       [Amazon ECS & Fargate Master Class — Docker on AWS (Udemy)](https://www.udemy.com/course/aws-ecs-fargate/) — closest match to the ECS/Fargate deployment this day needs

•       [Amazon ECS for DevOps: Docker, CI/CD & Deployments Hands-On (Udemy)](https://www.udemy.com/course/elastic-container-service-ecs-aws-devops-docker-2025/) — alternative, more CI/CD-flavored

•       [Ultimate AWS Certified Developer Associate (Udemy)](https://www.udemy.com/course/aws-certified-developer-associate-dva-c01/) — best single reference for IAM, CloudWatch, RDS, and Secrets Manager specifically

•       AWS Skill Builder (free official training): <https://skillbuilder.aws> — search "ECS," "IAM," and "CloudWatch" digital courses

•       Databricks on AWS reference architecture: <https://docs.databricks.com/aws/en/>

## **Day 16 — PostgreSQL: schema & data-layer design**

Since this team already knows SQL, the bar here is schema and data-layer design, not just knowing how to call an ORM: domain modeling and normalization tradeoffs, choosing keys and relationships/constraints deliberately, designing the schema the capstone will actually run on, and a migration strategy for evolving it safely. Once the schema is designed, spend the back half of the day on EXPLAIN ANALYZE and query planning, index types (B-tree, GIN, partial, composite), and transactions/isolation levels — the parts that are easy to get wrong once ORM-level familiarity is assumed rather than checked.

Hands-on: design the Postgres schema for the capstone domain from scratch (not generated by the ORM), then profile and optimize two or three queries against it.

Resources:

•       [SQL and PostgreSQL: The Complete Developer's Guide (Udemy, Stephen Grider)](https://www.udemy.com/course/sql-and-postgresql/) — has a strong "performance and internals" section that suits this audience; skip the beginner SQL modules

•       Official docs: <https://www.postgresql.org/docs/current/> — specifically the chapters on indexes, performance tips, and concurrency control

## **Days 17-20 — Capstone build and demo**

**Day 17:** build the core end-to-end slice: a Flutter screen that calls a NestJS service over gRPC, which reads/writes Postgres. Get this working cleanly end-to-end before adding the ML/data layer on Day 18 — don't let debugging gRPC plumbing eat into the next day's work.

**Day 18:** wire in the Python/FastAPI model-serving service from Day 13 and the Databricks job from Day 14 so the feature calls the model for a prediction and is fed by Databricks' aggregated Postgres data. Use Cursor's agent mode deliberately throughout, since fluency with it is as much a goal of this course as the frameworks are.

**Day 19:** finish integration and deploy the NestJS and Python services to AWS ECS with IAM roles and CloudWatch wired up. Days 17-18 are the two riskiest days in the whole plan (gRPC plumbing, then ML wiring), so if either ran long, use whatever's left of Day 19 to catch up before moving to deployment rather than rushing it.

**Day 20 (buffer, demo, and retro):** a dedicated buffer day, added specifically because the team flagged the capstone block as tight. Use the morning to close out anything still open from Days 17-19; then demo the feature to yourself end-to-end against a short checklist. Close with a written self-retro — what felt intuitive, what needs another day of practice, and which of the two ORM/state-management choices from Days 4 and 7 you'd standardize on — and share it async with the rest of the team if others are taking this course too, rather than scheduling a group session for it.

## **Reference materials by technology (consolidated)**

**Flutter:** [Flutter & Dart – The Complete Guide](https://www.udemy.com/course/learn-flutter-dart-to-build-ios-android-apps/) · [Flutter Bootcamp with Dart](https://www.udemy.com/course/flutter-bootcamp-with-dart/) · [docs.flutter.dev](https://docs.flutter.dev) · [dart.dev/language](https://dart.dev/language)

**NestJS:** [NestJS: The Complete Developer's Guide](https://www.udemy.com/course/nestjs-the-complete-developers-guide/) · [NestJS Ultimate Masterclass](https://www.udemy.com/course/nestjs-ultimate-masterclass-basics-to-advanced-2026/) · [docs.nestjs.com](https://docs.nestjs.com)

**gRPC / Protocol Buffers:** [gRPC [Node.js] MasterClass](https://www.udemy.com/course/grpc-nodejs/) · [NestJS gRPC microservices guide](https://docs.nestjs.com/microservices/grpc) · [grpc.io docs](https://grpc.io/docs/) · [protobuf.dev](https://protobuf.dev/programming-guides/proto3/)

**PostgreSQL:** [SQL and PostgreSQL: The Complete Developer's Guide](https://www.udemy.com/course/sql-and-postgresql/) · [postgresql.org/docs](https://www.postgresql.org/docs/current/)

**Databricks:** [Databricks free self-paced training](https://www.databricks.com/learn/training/home) · [Databricks Free Edition](https://www.databricks.com/learn/free-edition) · [Databricks Certified Data Engineer Associate Bootcamp](https://www.udemy.com/course/databricks-certified-data-engineer-associate-bootcamp/) · [docs.databricks.com](https://docs.databricks.com)

**Cursor:** [Cursor Course: FullStack Development with Cursor](https://www.udemy.com/course/cursor-ai-ide/) · [docs.cursor.com](https://docs.cursor.com) · [Cursor AI: The Only IDE You Need in 2026 (YouTube)](https://www.youtube.com/watch?v=ed26g7jxAbU)

**BMAD Method & BMAD TEA (Test Architect):** [Official BMAD docs](https://docs.bmad-method.org) · [GitHub — BMAD-METHOD](https://github.com/bmad-code-org/BMAD-METHOD) · [TEA docs](https://bmad-code-org.github.io/bmad-method-test-architecture-enterprise/) · [GitHub — TEA](https://github.com/bmad-code-org/bmad-method-test-architecture-enterprise) 

**AWS:** [Amazon ECS & Fargate Master Class](https://www.udemy.com/course/aws-ecs-fargate/) · [Amazon ECS for DevOps](https://www.udemy.com/course/elastic-container-service-ecs-aws-devops-docker-2025/) · [Ultimate AWS Certified Developer Associate](https://www.udemy.com/course/aws-certified-developer-associate-dva-c01/) · [AWS Skill Builder (free)](https://skillbuilder.aws) · [Databricks on AWS docs](https://docs.databricks.com/aws/en/)

**Python / AI-ML services:** [The Complete Python Bootcamp](https://www.udemy.com/course/complete-python-bootcamp/) · [docs.python.org tutorial](https://docs.python.org/3/tutorial/) · [FastAPI – The Complete Course 2026](https://www.udemy.com/course/fastapi-the-complete-course/) · [Deploy ML Model in Production with FastAPI and Docker](https://www.udemy.com/course/deploy-ml-model-in-production-with-fastapi-and-docker/) · [fastapi.tiangolo.com](https://fastapi.tiangolo.com)

