# Codebase Overview

This document summarizes the high level architecture and how the repository is organized. It is intended to help contributors navigate the project quickly.

## Architecture

- **Framework**: Ruby on Rails 7.2 with the Hotwire suite (Turbo and Stimulus) providing the reactive front‑end.
- **Persistence**: PostgreSQL database.
- **Background Jobs**: Sidekiq workers backed by Redis. Recurring jobs are defined in `config/schedule.yml` and managed with `sidekiq-cron`.
- **External Providers**: Optional integrations for Plaid (bank data), Stripe (billing), Synth (market data and exchange rates) and OpenAI (AI features). Providers are registered dynamically through `app/models/provider/registry.rb` so self‑hosted installs can enable only the required services.

## Code Layout

```
app/            main Rails application code
  assets/       static assets managed by the asset pipeline
  channels/     Action Cable channels
  components/   reusable view components
  controllers/  HTTP controllers and API endpoints
  helpers/      view helpers
  javascript/   Stimulus controllers and other JS
  jobs/         ActiveJob and Sidekiq workers
  mailers/      mailer classes
  models/       domain models
  views/        view templates
bin/            command line scripts (setup, dev, rake wrappers)
config/         application configuration, routes and initializers
  environments/ environment specific settings
  schedule.yml  cron style schedule for Sidekiq jobs
  routes.rb     web routes
  storage.yml   Active Storage configuration
  ...
db/             database migrations and seeds
  migrate/      schema migrations
  schema.rb     canonical database schema
  seeds/        seed files for demo or initial data
lib/            standalone modules and rake tasks
public/         static files served directly by Rails
docs/           project guides and architecture explanations
test/           Minitest test suite (unit, system, etc.)
```

## Key Domain Relationships

The core domain revolves around families, accounts, transactions and budgets. Important associations include:

- **Family** – top level entity that `has_many` users, accounts, budgets and categories. Transactions are accessible through associated accounts.
- **Account** – belongs to a family and `has_many` entries, transactions, valuations and holdings. Accounts are syncable via external providers.
- **Transaction** – represents inflows or outflows on an account, belongs to a category and is linked to an entry.
- **Budget** – monthly plan belonging to a family. It `has_many` budget_categories that track allocations for each expense category.

See `docs/core_models.md` for more details on the domain models and `db/schema.rb` for the full schema.

## Getting Started

For development setup instructions consult `docs/setup.md`. To self‑host the application using Docker see `docs/hosting/docker.md`.

## Running Tests

- The project uses Minitest for unit, integration and system tests.
- Run unit and integration tests with `bin/rails test`.
- Run system tests with `bin/rails test:system`. In CI, `DISABLE_PARALLELIZATION=true` is set to avoid concurrency issues.
- Set `COVERAGE=true` to generate SimpleCov reports.
- `.env.test.example` documents other environment variables like provider keys for the test suite.
