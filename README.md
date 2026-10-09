# AI Tax Copilot (FY 2025-26 / AY 2026-27)

An AI-powered personal chartered accountant for Indian taxpayers — combining a deterministic tax engine, immutable scenario snapshots, document intelligence, and a clean Next.js interface.

---

## What This Is

A 3-service architecture where every rupee is computed by tested code and every explanation comes from AI. The AI never does arithmetic. The engine never guesses.

| Service | Directory | Owner | Port | Role |
| :--- | :--- | :---: | :---: | :--- |
| **`tax-engine`** | `src/` / `prisma/` | **Member 1** | `3000` | Deterministic tax computation & immutable Twin state |
| **`ai-backend`** | `ai-backend/` | **Member 2** | `3002` | Gemini orchestration, document intelligence (AIS / 26AS / Form 16), citations |
| **`frontend`** | `frontend/` | **Member 3** | `3001` | Next.js interface — consumes backend payloads, performs zero math |
| **`postgres`** | `docker/` | Infrastructure | `5433` | PostgreSQL 16 — relational store |

---

## Running Locally

One command brings up all four containers:

```bash
docker compose up --build
```

Once the logs settle, open:

- **App** → [http://localhost:3001](http://localhost:3001)
- **Tax Engine** → [http://localhost:3000](http://localhost:3000)
- **AI Backend** → [http://localhost:3002](http://localhost:3002)

---

## Demo Data & Tests

```bash
# Load a sample taxpayer (Form 16 + AIS)
docker compose run --rm tax-engine npx tsx src/scripts/seed.ts

# Run the full regression suite
docker compose run --rm tax-engine npm test
```

---

## Design Rules

1. **Single source of truth.** Every Twin is versioned ($v_1 \rightarrow v_2 \rightarrow v_3$). Snapshots are immutable — scenarios fork, they never mutate.
2. **Numbers from code, words from AI.** Gemini reads documents and explains results. It never calculates. Every rupee figure it speaks comes from a tool call into the engine.
3. **No math in the browser.** The frontend renders what the backend returns. Nothing more.
4. **Secrets stay server-side.** API keys and database credentials never reach the client.
