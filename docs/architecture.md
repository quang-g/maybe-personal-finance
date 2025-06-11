# Architecture Overview

This document gives a brief overview of the major components that make up Maybe.

The application is a standard Ruby on Rails monolith that also serves a modern JavaScript front end. Postgres is used as the primary database and Redis powers background jobs and caching. Docker is the recommended way to run these services together in development and production environments.

