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
