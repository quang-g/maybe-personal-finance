
# Local Development Setup

This guide walks through getting the app running on your machine for development.

## Prerequisites

- Ruby `3.4.4`
- PostgreSQL (version 9.3 or later)
- Redis

## Getting Started

Clone the repository and run the following commands from the project root:

```bash
cp .env.local.example .env.local
bin/setup
bin/dev
```

The application will be available at <http://localhost:3000>.

### Optional Demo Data

If you would like to preload some demo data, run:

```bash
rake demo_data:reset
```

## Docker

For information on using Docker to self‑host the app, see [our Docker guide](hosting/docker.md).
=======
# Quick Setup Guide

Follow these basic steps to get Maybe running on your machine.

1. Clone the repository.
2. Copy `.env.local.example` to `.env.local` and adjust settings as needed.
3. Run `bin/setup` followed by `bin/dev` to start the application.

For self-hosting instructions see [the Docker guide](hosting/docker.md).

