# 3PD Astro Forms Starter Kit

A starter kit for building Astro micro-frontends with forms and data persistence, deployed as Drupal block modules inside the HUDX platform.

## What is this?

HUDX is a Drupal-based platform. Third-party developers (3PDs) extend it by building self-contained applications using one of the 3PD starter kits. Each app gets packaged as a Drupal module — no Drupal development experience required.

This starter is for apps that need to **collect and store data** (forms, submissions, user input). It combines Astro for the UI with an Express + SQLite backend for local development. In production, form data goes directly into Drupal's database.

**When to use this starter:**
- Your app needs a form that saves data
- You want to display Drupal content AND collect user input
- You need a simple, lightweight frontend (no full React SPA)

**When to use a different starter:**
- Display-only content from Drupal, no forms → use the [Astro Static starter](https://github.com/sweimer/3PD---Astro-Static-Starter-Kit)
- Complex React UI with client-side routing and state → use the [React starter](https://github.com/sweimer/3PD---React-Starter-Kit)

---

## How it works

| Environment | Frontend | API | Data |
|---|---|---|---|
| Local dev | Astro at `localhost:4321` | Express at `localhost:3001` | SQLite file |
| Deployed in Drupal | Astro bundle loaded as a Drupal library | PHP controller (auto-generated) | Drupal MySQL DB |

You build and test locally against Express + SQLite. When ready, one CLI command builds and packages everything as a Drupal module. The HUDX team installs it.

---

## Prerequisites

- Node.js v20+
- npm
- The 3PD CLI — see [3pd-ide](https://github.com/sweimer/sandbox-glazed) for installation instructions

You do **not** need Drupal, Lando, or Docker to develop locally.

---

## Getting started

**1. Get the starter**

Your HUDX project lead will scaffold your app using the 3PD CLI. If you are cloning this repo directly to explore:

```bash
git clone https://github.com/sweimer/3PD---Astro-Forms-Starter-Kit.git my-app
cd my-app
npm install
cp .env.example .env
```

**2. Pull current data** (if your app is already deployed)

Sync your local SQLite with live data before starting work:

```bash
3pd astro-forms db pull
```

**3. Start the dev server**

```bash
npm run dev
```

Opens two servers:
- **Astro** at `http://localhost:4321` — your app UI
- **Express** at `http://localhost:3001` — local API for form submissions

Edit files in `src/` — hot reload is active.

---

## Project structure

```
your-app/
├── src/
│   ├── layouts/
│   │   └── Layout.astro    ← HTML shell
│   └── pages/
│       └── index.astro     ← Start here — your app UI + form
├── server/
│   ├── server.js           ← Express API (local dev only)
│   └── db/
│       ├── schema.sql      ← Define your data table here
│       └── app.sqlite      ← Local data (gitignored)
├── .env                    ← Local config (gitignored)
├── .env.example            ← Commit this — no secrets
└── package.json
```

**Where to start building:** `src/pages/index.astro` for the UI and form, `server/db/schema.sql` for your data shape, `server/server.js` for your API endpoints.

---

## Packaging for Drupal

When your feature is ready to hand off:

```bash
3pd astro-forms module
```

This builds the Astro app and generates a Drupal module folder in your app directory. Commit that folder and push your feature branch. The HUDX team handles installation.

> **Do not run** `3pd astro-forms module --install` — that flag requires internal Drupal access and is for HUDX team use only.

---

## Important gotchas

- **Do not edit `APP_SLUG` in `.env`.** It is set when your app is scaffolded and must stay consistent.
- **`PUBLIC_` prefix is required** for any env var you need in the Astro frontend (e.g. `PUBLIC_API_BASE_URL`).
- **The Express server is local only.** In production, API routes are handled by an auto-generated PHP controller. Keep your Express routes and schema in sync.
- **Schema changes after deployment** require the HUDX team to update the Drupal database — coordinate before changing field names or types.
