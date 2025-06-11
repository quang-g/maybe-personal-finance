# “Re‑create *Maybe* from Zero” – Full‑Stack Rails 7.2 Course

**Course goal—** Every graduate can spin up a blank Rails repo and, commit‑by‑commit, rebuild the open‑source personal‑finance app that lives in [`maybe-finance/maybe`](https://github.com/maybe-finance/maybe). The finished clone behaves identically to the original: same commands (`bin/setup`, `bin/dev`), same demo data, same feature set.

---

## Weekly Roadmap & Tagged Releases

| Week | Release tag | Milestone – what students build **from scratch** | Key concepts introduced | Visible proof‑point |
|------|-------------|-----------------------------------------------|-------------------------|-----------------------|
| 0 | `v0.1-bootstrap` | Fresh Rails 7.2 app with Hotwire; Postgres & Redis via Docker Compose | Gemfile hygiene, dev containers | `rails s` → “Welcome” page |
| 1 | `v0.2-core-domain` | Migrations & models for **Family, User, Account, Transaction, Budget** | ER modelling, UUID PKs, enums | `rails c` shows schema & associations |
| 2 | `v0.3-auth-multitenant` | Devise + Family scoping, invite flow | Multi‑tenant patterns, Pundit policies | Sign‑up → dashboard scoped to one Family |
| 3 | `v0.4-transactions-ui` | CRUD for Accounts & Transactions with Turbo Streams | RESTful controllers, Hotwire | Live‑updating ledger table |
| 4 | `v0.5-budgeting` | Monthly envelope engine & rollover rules | Service objects, business‑rule layer | “Budget vs Actual” widget |
| 5 | `v0.6-multi-currency` | `money-rails` setup, Synth rates adapter | Money math, external API wrappers | JPY/EUR amounts convert correctly |
| 6 | `v0.7-provider-registry` | `Provider::Registry` and Plaid skeleton | Plug‑in architecture, POROs | Fake Plaid import creates transactions |
| 7 | `v0.8-background-jobs` | Sidekiq integration, cron YAML | Redis queues, retries, idempotency | `PlaidSyncJob` enqueued hourly |
| 8 | `v0.9-json-api` | Versioned `/api/v1/*` controllers & serializers | Pagination, JSON API standards | `curl` returns paginated JSON |
| 9 | `v1.0-subscriptions` | Stripe Checkout & webhook handling for plans | Billing flows, secure webhooks | Test card → live “Pro” flag |
| 10 | `v1.1-tests-green` | RSpec, FactoryBot, VCR suites ≥ 80 % coverage | TDD in Rails, CI mindset | `bundle exec rspec` all‑green |
| 11 | `v1.2-observability` | Bullet, Skylight, structured logging | N+1 busting, perf budgets | Skylight snapshot in README |
| 12 | `v1.3-ci-docker` | GitHub Actions build + multi‑stage Dockerfile | Containerisation, Continuous Delivery | `docker compose up` works in CI |
| 13 | `v1.4-frontend-polish` | Stimulus controllers, lazy‑loaded charts | Progressive enhancement | Instant‑update net‑worth chart |
| 14–15 | `v1.5-capstone` | Student‑chosen feature (Goals, Retirement, AI chat, etc.) | Rails Engines, modular monolith | Demo & PR merged into course fork |

---

## Teaching Mechanics

* **Cadence**: 2 h recorded theory · 2 h live coding · 2 h lab each week.  
* After every week students push a **tagged release**; an autograder runs the same smoke tests shipped with the real *Maybe* repo.  
* Starter repo contains empty folders & failing specs—learners make them pass until their codebase is indistinguishable from upstream.

## Prerequisites

* Working knowledge of Ruby, Git, basic SQL, and command‑line tools.  
* Laptop with Docker Desktop or GitHub Codespaces account.

## Graduation Checklist (all must be true)

1. `bin/setup && bin/dev` launches the rebuilt app exactly as documented in the original README.  
2. Demo data task seeds login `user@maybe.local / password`.  
3. CI pipeline builds, runs all tests, and publishes a production‑ready Docker image.  
4. Feature parity confirmed by a diff tool that compares routes, schema, and key views with the source repo.

## Outcomes for Students

* Deep understanding of a modern Rails 7.2 stack—Hotwire, Sidekiq, Stripe, external API adapters.  
* Ability to architect, test, containerise, and deploy a full‑stack SaaS from scratch.  
* A public portfolio project: their own fully‑functional *Maybe* clone, ready to fork, extend, or host commercially.

