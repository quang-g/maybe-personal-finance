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

## Coding Standards

These guidelines keep the codebase consistent and easy to understand:

- **Ruby style** is enforced through [RuboCop](https://github.com/rubocop/rubocop) using the `rubocop-rails-omakase` preset. Indentation uses **two spaces** and tabs are never used. The config lives in `.rubocop.yml`.
- **ERB templates** are checked by `erb_lint` and require **double quoted** strings.
- **JavaScript** in `app/javascript` is formatted and linted with [Biome](https://biomejs.dev) (`npm run lint`). The formatter also enforces two‑space indentation and double quotes.
- A project wide `.editorconfig` sets UTF‑8 encoding, LF line endings and two‑space indentation so editors stay in sync.
- Class names follow `CamelCase` while files, methods and variables use `snake_case`. Stimulus controllers and other JS files are named with `snake_case` as well.
- Use `Current.user` and `Current.family` for global context inside controllers and models.

### Development practices

- Place business logic in `app/models` using plain Ruby objects or Rails concerns. Avoid separate `services` directories.
- Tests use **Minitest** with a minimal fixture set. Helpers in `test/support` act as lightweight factories when many records are required.
- Stick to the [Tailwind](app/assets/tailwind/maybe-design-system.css) tokens when writing styles and prefer semantic HTML with Turbo/Stimulus.

## Running the Test Suite

Tests use **Minitest** along with fixtures and helpers in `test/support`.

1. Install dependencies:
   ```
   bundle install
   npm install
   ```
2. Copy the example environment file:
   ```
   cp .env.test.example .env.test
   ```
3. Set up the database:
   ```
   bin/rails db:create db:schema:load
   ```

Run tests with:

```
bin/rails test                         # all unit and integration tests
DISABLE_PARALLELIZATION=true bin/rails test:system  # system tests (needs Chrome)
COVERAGE=true bin/rails test           # enable SimpleCov
```

Optional environment variables include `DISABLE_PARALLELIZATION`, `COVERAGE` and `E2E_BROWSER`. See `test/test_helper.rb` for details.
