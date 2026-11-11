---
name: node-express-backend
description: Scaffold a Node.js + Express + TypeScript backend (structure, config, health check, tests and an AGENTS.md; no features). Use when asked to create a new backend/API project. Includes a .env.example (with PORT) to be copied to .env.
---

# Node + Express backend

## Steps

1. Ask the user where to create the project: in the current root folder, or in
   a new subfolder (ask for its name, suggesting `backend`). Ask this together
   with the goal question below, then wait for the answer. Do not create any
   file, folder or run any command until the user has answered.
   All later paths and commands are relative to the chosen folder (`cd` into
   the subfolder after creating it).
2. Ask the user for the project goal in 1 sentence. It is only written into
   AGENTS.md; it does not authorize building anything. If the user doesn't
   give one, use "TBD" instead of inferring it.
3. Create AGENTS.md FIRST using the template below, filling in the goal.
4. Scaffold the project following "Structure" and "Standards". No features yet.
5. Create `.env.example` (with PORT), copy it to `.env`, and add `.env` to `.gitignore`.
6. Add one health-check test and run `npm test` to confirm setup.
7. Stop. Report what was created and the test result, then ask the user which
   feature to build first. Do not add features, endpoints, pages or
   components the user did not explicitly name.

## Done when

- Files exist exactly as in "Structure"; routes/ contains only health.ts,
  services/ and schemas/ are empty.
- `GET /health` returns `{ "status": "ok" }`.
- `npm test` (the single health test) and `npm run typecheck` pass.
  Nothing beyond this. Stop here.

## Structure

```
.env.example      # committed template (copy to .env)
.env              # local, git-ignored
src/
  config.ts       # reads and validates env vars
  app.ts          # creates and exports the Express app (no listen)
  server.ts       # imports app and calls listen
  routes/         # route definitions only
  services/       # business logic (pure, testable)
  schemas/        # zod schemas for input validation
  middleware/
    error.ts      # central error handler
tests/            # vitest + supertest
```

## Standards

- TypeScript, strict mode.
- Use ESM ("type": "module", tsconfig "module": "NodeNext") and import local files with the .js extension.
- Deps: express, zod. Dev deps: typescript, tsx, vitest, supertest, @types/\*.
- Scripts: `dev` (tsx watch src/server.ts), `build` (tsc), `test` (vitest run), `typecheck` (tsc --noEmit).
- Expose `GET /health` returning `{ "status": "ok" }` (used by the frontend example).
- Validate all request input (body, params, query) with zod and return 400 on invalid input.
- Routes stay thin: parse → call service → respond.
- Central error middleware; never leak stack traces.
- Correct status codes: 200, 201, 204, 400, 404, 500.
- In-memory store unless told otherwise; no DB, auth or external services by default.
- Test via supertest against `app` (not the server).

## Environment

- `.env.example` must list every env var with safe defaults, e.g.:
  ```
  PORT=3000
  NODE_ENV=development
  ```
- Load env with Node's built-in flag (no dotenv): `dev` = `tsx watch --env-file=.env src/server.ts`.
- `src/config.ts` reads `process.env`, validates with zod and exports a typed `config`; no other file reads `process.env` directly.
- `server.ts` listens on `config.port`.
- `.env` is git-ignored; never commit secrets.

Example `src/config.ts`:

```ts
import { z } from "zod";

const schema = z.object({
  PORT: z.coerce.number().default(3000),
  NODE_ENV: z
    .enum(["development", "test", "production"])
    .default("development"),
});

const env = schema.parse(process.env);

export const config = { port: env.PORT, nodeEnv: env.NODE_ENV };
```

## AGENTS.md template

```md
# AGENTS.md

Goal: <1 sentence>
Stack: Node + Express + TypeScript, zod, Vitest + supertest
Map: src/app.ts (app), src/routes (HTTP), src/services (logic), src/schemas (validation), tests/
Commands: npm run dev | npm test | npm run typecheck
Loop: understand → write failing test → smallest fix/feature → npm test && npm run typecheck
Rules:

- Small diffs; one feature or fix at a time.
- Validate all client input with zod (trust boundary).
- No new deps, DB, auth or external services without asking.
- Don't modify unrelated code or delete existing tests.
- Every bug fix gets a regression test.
- Config via env vars only (see .env.example); new vars must be added to .env.example and src/config.ts.
```
