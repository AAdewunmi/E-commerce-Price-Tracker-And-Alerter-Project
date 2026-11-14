# Droply: Automated E-commerce Price Tracker & Alerting Platform

[![Status: Work in Progress](https://shields.io)](#project-status--roadmap)
[![CI Pipeline](https://shields.io)](#local-development--verification)
[![Code Style: Ruff](https://shields.io)](https://github.com)
[![Python](https://shields.io)](https://python.org)
[![FastAPI](https://shields.io)](https://tiangolo.com)
[![PostgreSQL](https://shields.io)](https://postgresql.org)
[![Docker](https://shields.io)](https://docker.com)
[![License: MIT](https://shields.io)](LICENSE)

> **Note:** Droply is currently under active development. Core architectural foundations, database schemas, and initial API endpoints are being rolled out iteratively. Features and interfaces are subject to change until `v1.0.0`.

Droply is a full-stack web application designed to track e-commerce product prices on an automated schedule, store historical trends, and send email alerts when prices cross below user-defined target thresholds. It couples a zero-reload HTMX and Chart.js interface with an asynchronous FastAPI, Celery, and PostgreSQL backend.

---

## Project Status & Roadmap

This repository is an active work in progress. Development is structured into five incremental milestones:

* [x] **Milestone 1: Foundations & Architecture** — Repository setup, Docker Compose orchestration, FastAPI app structure, Ruff linting, and PostgreSQL schema design.
* [ ] **Milestone 2: Authentication & Watchlist Core** — JWT cookie authentication, user registration/login, and deduplicated `UserWatches` CRUD endpoints. *(In Progress)*
* [ ] **Milestone 3: Smart Scraper & Celery Workers** — Strategy-pattern scraper (`HTTPX` + `BeautifulSoup4` with `Playwright` fallback), exponential backoff retries, and periodic Celery Beat scheduling.
* [ ] **Milestone 4: Interactive Dashboard & Alerting** — HTMX live product dashboard, Chart.js 90-day price history charts, one-click Recruiter Demo Mode, and idempotent SendGrid email alerts.
* [ ] **Milestone 5: Testing, CI/CD & Deployment** — Comprehensive `pytest` async suite (`>=80%` coverage), GitHub Actions CI gates, Sentry monitoring, and cloud deployment.

---

## Planned Core Capabilities

* **Full-Stack UI:** Zero-reload dashboard powered by HTMX, interactive 90-day price trends with Chart.js, JWT cookie authentication, and a one-click Recruiter Demo Mode pre-populated with sample data.
* **Smart Scraping & Alerting:** Combines fast static extraction (`HTTPX` + `BeautifulSoup4`) with headless fallback (`Playwright`), exponential backoff for anti-blocking, deduplicated watch architecture, and idempotent SendGrid alerts.

---

## Tech Stack & Architecture

* **Backend & API:** Python 3.12+, FastAPI, SQLAlchemy 2.0 (Async), Pydantic v2, Alembic
* **Frontend:** Jinja2 Templates, Tailwind CSS, HTMX, Alpine.js, Chart.js
* **Scraping & Workers:** HTTPX, BeautifulSoup4, Playwright (Chromium), Celery, Celery Beat, Redis 7
* **Database & Ops:** PostgreSQL 16, Docker, Docker Compose, Gunicorn, Uvicorn, Nginx, SendGrid API, GitHub Actions

```text
[Browser (HTMX + Chart.js)] ──► [Nginx] ──► [FastAPI] ──► [PostgreSQL 16]
                                                    │
                                                    ▼
   [Celery Beat] ──► [Celery Workers] ──► [HTTPX / Playwright] & [SendGrid]
```

---

## Quick Start (Development Environment)

1. **Clone and Configure:**
   ```bash
   git clone https://github.com
   cd Droply-Ecommerce-Price-Tracker
   cp .env.example .env
   ```
2. **Build and Start Services:**
   ```bash
   make build && make up && make migrate && make seed
   ```
   * **Web App:** `http://localhost:8000` | **API Docs:** `http://localhost:8000/docs`

### Planned Demo Accounts *(Available upon Milestone 4 completion)*
* **Populated Demo User:** `demo@droply.local` / `DroplyDemoPass123`
* **Empty-State User:** `newuser@droply.local` / `DroplyDemoPass123`

---

## Local Development & Verification

Standard development and quality-gate commands are available via the `Makefile`:
* `make up` / `make logs` / `make migrate` / `make seed`
* `make test` (pytest with async support), `make test-cov` (>=80% coverage check), `make lint` (Ruff), and `make check` (full quality gate).

The repository structure cleanly separates API handlers (`app/api/`), ORM models (`app/models/`), scraper strategies (`app/scraper/`), background workers (`app/worker/`), and automated GitHub Actions CI pipelines (`.github/workflows/ci.yml`).

---

## License

Licensed under the MIT License. See [LICENSE](LICENSE) for details.
