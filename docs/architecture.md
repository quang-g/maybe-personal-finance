# Architecture Overview


This project is built with **Ruby on Rails 7.2** and uses the Hotwire suite (Turbo
and Stimulus) for the front‑end. Data is stored in **PostgreSQL** and background
jobs run through **Sidekiq** backed by **Redis**.

## External Providers

Several external services power optional features:

- **Plaid** – bank data aggregation
- **Stripe** – subscription billing
- **Synth** – market data and exchange rates
- **OpenAI** – AI features such as chat

These providers are registered at runtime via
`app/models/provider/registry.rb`, allowing self‑hosted installs to pick and
choose which services to enable.

## Core Domain Models

- **Family** – top level entity that owns users and accounts
- **Account** – represents a single financial account with balances and entries
- **Transaction** – entry that tracks income or spending on an account
- **Budget** – monthly budget with categories and allocations

Refer to `db/schema.rb` for the full relationships.

## Job Scheduling

Background jobs are queued with Sidekiq. Recurring tasks are configured in
`config/schedule.yml`, which is loaded by `sidekiq-cron` to enqueue jobs on a
schedule. Examples include market data imports and cleanup tasks.
=======
This document gives a brief overview of the major components that make up Maybe.

The application is a standard Ruby on Rails monolith that also serves a modern JavaScript front end. Postgres is used as the primary database and Redis powers background jobs and caching. Docker is the recommended way to run these services together in development and production environments.
