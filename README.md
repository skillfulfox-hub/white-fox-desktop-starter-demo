# White Fox: Ultimate Desktop Starter Kit

Commercial desktop starter kit built with `Electron + Angular + NestJS + Nx`.

`White Fox` helps you launch production-minded desktop applications faster with a modular monorepo, authentication starter flow, API docs, and ready-to-package distribution setup.

## Product Positioning

This repository is a **commercial starter kit** designed for developers, studios, and agencies who want to:

- Build cross-platform desktop products with a modern TypeScript stack
- Reuse a proven monorepo architecture instead of starting from scratch
- Ship faster with prewired frontend, backend, authentication, and packaging

## Key Value

- End-to-end stack in one workspace: Electron shell + Angular UI + NestJS API
- Monorepo productivity with Nx task orchestration and scalable project layout
- Built-in authentication baseline (register/login + JWT)
- Database-ready backend (SQLite, PostgreSQL, MongoDB modes)
- Swagger docs out of the box for fast API integration
- Electron Builder configuration included (Windows NSIS target)

## Core vs Advanced

Use this starter in two layers:

- **Core (recommended for first shipping version):**
  secure Electron baseline, auth starter, API process bootstrap, packaging, lint/test CI, packaged smoke test.
- **Advanced (optional, enable when needed):**
  crash reporter uploads, release channels, custom readiness tuning, deeper release automation.

If you are selling a v1 product, focus on **Core first** and keep Advanced settings disabled.

## Tech Stack

- Monorepo: Nx `22.x`
- Desktop: Electron `40.x`
- Frontend: Angular `21.x`, Angular Material, SCSS
- Backend: NestJS `11.x`, TypeORM
- Language: TypeScript `5.9`
- Testing: Jest, Vitest, Playwright
- Packaging: electron-builder

## What Is Included

### Frontend (`apps/client-app`)

- Angular standalone app architecture
- Auth screens: Login / Register
- App shell + header layout
- Home feature with basic user management UI flow
- HTTP interceptor and core service layer

### Backend (`apps/server-api`)

- NestJS modular architecture
- Auth module (`/api/auth/register`, `/api/auth/login`)
- Users module (`/api/users` CRUD subset, JWT-protected)
- JWT support
- TypeORM integration with environment-driven DB provider
- Swagger docs at runtime (`/api/docs`)

### Desktop Shell (`apps/electron-shell`)

- Electron main process with preload bridge
- Runtime API process bootstrap from Electron
- Dynamic API port allocation
- API readiness handshake before renderer load
- Environment-driven app title and window size
- Structured JSON logs for startup/runtime failures
- Optional crash reporter setup with release channel metadata
- Packaging configuration for distributable builds

## Architecture Overview

```text
Nx Monorepo
+- apps/client-app      -> Angular renderer (UI)
+- apps/server-api      -> NestJS API (auth/users + docs)
+- apps/electron-shell  -> Electron main/preload + app packaging
```

At development runtime, Electron starts the backend process and loads the Angular client (dev server URL or packaged static build).

## Prerequisites

- Node.js `20+` (recommended LTS)
- npm `10+`
- Windows/macOS/Linux development environment

## Quick Start

### 5-Minute Onboarding

```bash
npm install
npx nx serve electron-shell
```

This starts the full desktop development flow:

- Angular client dev server
- Electron shell
- NestJS API process (spawned by Electron)

## Starter Commands (Core)

```bash
# Full desktop dev mode
npx nx serve electron-shell

# Frontend only
npx nx run client-app:serve

# Backend only
npx nx run server-api:serve

# Build packaged application (electron-builder)
npx nx run electron-shell:make
```

## Root npm Scripts

```bash
# Main desktop development flow
npm run dev

# Build all deployable apps
npm run build

# Unit tests (frontend + backend)
npm run test

# End-to-end checks
npm run e2e
npm run e2e:packaged

# Build distributable package
npm run package
```

## Quality and Release Commands

```bash
# Lint frontend
npx nx run client-app:lint

# Backend unit tests
npx nx run server-api:test

# Frontend unit tests
npx nx run client-app:test

# End-to-end tests (Playwright)
npx nx run client-app:e2e
npx nx run electron-shell-e2e:e2e
npx nx run electron-shell-e2e:e2e-packaged

# Visualize workspace graph
npx nx graph
```

## Environment Configuration

Project uses root `.env`.

### Core Variables (Required)

```env
# Database
DB_TYPE=sqlite
DB_NAME=database.sqlite

# API
API_PORT_RANGE_START=3000

# Client / App
CLIENT_DEV_URL=http://localhost:4200
APP_TITLE="White Fox"

# Electron Window
WINDOW_WIDTH=1280
WINDOW_HEIGHT=800

# Auth
JWT_SECRET=super-secret-key-change-me
JWT_EXPIRES_IN=24h
```

### Advanced Variables (Optional)

```env
# Readiness tuning
API_HEALTHCHECK_PATH=/api/get-status
API_READY_TIMEOUT_MS=20000
API_READY_RETRY_INTERVAL_MS=300

# Release metadata
APP_CHANNEL=stable
APP_ENV=production

# API docs / CORS
SWAGGER_ENABLED=true
CORS_ALLOWED_ORIGINS=http://localhost:4200,http://127.0.0.1:4200

# Database behavior
DB_SYNCHRONIZE=false
DB_LOGGING=none

# Crash Reporting
CRASH_REPORTER_ENABLED=false
CRASH_REPORTER_UPLOAD_URL=
```

### Database Modes

Set `DB_TYPE` to one of:

- `sqlite` (default, local file)
- `postgres` (requires `DB_HOST`, `DB_PORT`, `DB_USER`, `DB_PASS`, `DB_NAME`)
- `mongodb` (uses `DB_URL`)

`DB_LOGGING` modes:

- `none` / `false` / `0` -> disable SQL logging
- `full` -> enable full SQL logging
- unset -> warnings/errors only

## API Documentation

Swagger is enabled in backend bootstrap (including production by default).

- Base prefix: `/api`
- Docs path: `/api/docs`

Runtime URL example:

- `http://localhost:3000/api/docs` (or next free port started by Electron from `API_PORT_RANGE_START`)

In desktop runtime, `FINAL_API_PORT` is injected by Electron automatically.

## Starter Auth Defaults

For demo bootstrap, backend seeds a default admin user if missing:

- Username: `admin`
- Password: `!admin`

Change this behavior before shipping your production product.

## Packaging and Distribution

Electron Builder config is located at:

- `apps/electron-shell/electron-builder.json`

Current setup:

- `productName`: `White Fox`
- Output directory: `dist/packages`
- Windows target: `nsis`

Build command:

```bash
npx nx run electron-shell:make
```

Packaged runtime smoke test (recommended for CI):

```bash
npx nx run electron-shell-e2e:e2e-packaged
```

## Operational Checklists

- Production readiness checklist: [PRODUCTION_CHECKLIST.md](./PRODUCTION_CHECKLIST.md)
- Release execution checklist: [RELEASE_CHECKLIST.md](./RELEASE_CHECKLIST.md)

## Recommended Hardening Before Production

- Replace default `JWT_SECRET`
- Restrict `CORS_ALLOWED_ORIGINS` to trusted frontend origins only
- Validate and sanitize all incoming request payloads
- Add role/permission checks to protected endpoints
- Add CI checks for lint, tests, and e2e

## License and Commercial Terms

This repository is distributed under a **Proprietary Commercial License**.

- See full terms: [LICENSE.md](./LICENSE.md)
- Source redistribution as a template/starter/boilerplate is prohibited
- Public repository hosting of source code is prohibited by license terms

## Author and Contact

- Author: **Skillful Fox Studio**
- Email: `skillfulfoxstudio[at]proton.me`
- GitHub: https://github.com/skillfulfox-hub

## Who This Kit Is For

- Solo founders building desktop SaaS MVPs
- Product teams needing a modern desktop baseline
- Agencies delivering client desktop apps under private code ownership

## Support

For licensing, business, and technical inquiries:

- If you encounter any bugs or have technical questions, please open an Issue in the private GitHub repository. For business inquiries, contact us via the marketplace.

