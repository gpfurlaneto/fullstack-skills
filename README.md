# fullstack-skills

Agent skills for scaffolding a fullstack project (frontend + backend). Each skill is a `SKILL.md` file that an AI coding agent (e.g. Cursor) follows to create a new project from scratch: structure, config, health check, tests and an example `.env.example`. No features are built.

## Skills

| File | Skill name | What it scaffolds |
| --- | --- | --- |
| `frontend-scaffold.md` | `nextjs-frontend` | Next.js (App Router) + TypeScript + Tailwind frontend |
| `backend-scaffold.md` | `node-express-backend` | Node.js + Express + TypeScript backend |

## Usage

### Cursor

1. Copy each file into your project as `.cursor/skills/<skill-name>/SKILL.md`:

   ```
   .cursor/skills/nextjs-frontend/SKILL.md      <- frontend-scaffold.md
   .cursor/skills/node-express-backend/SKILL.md <- backend-scaffold.md
   ```

2. Ask the agent to create a new frontend or backend project. It will ask where to create it (root or subfolder) and for a one-sentence project goal, then scaffold it.

### Claude Code

1. Copy each file as `SKILL.md` into a skills folder. Use `.claude/skills/` in your project, or `~/.claude/skills/` to make them available in all projects:

   ```
   .claude/skills/nextjs-frontend/SKILL.md      <- frontend-scaffold.md
   .claude/skills/node-express-backend/SKILL.md <- backend-scaffold.md
   ```

2. Start Claude Code in your project and ask it to create a new frontend or backend project (or run `/nextjs-frontend` or `/node-express-backend`). Claude will ask where to create it and for a one-sentence project goal.

### Claude (claude.ai / desktop app)

1. Put each skill in its own folder named after the skill, with the file renamed to `SKILL.md`, and zip the folder:

   ```
   nextjs-frontend/SKILL.md
   node-express-backend/SKILL.md
   ```

2. Go to **Settings > Capabilities > Skills**, upload each zip and enable it.
3. Ask Claude to create a new frontend or backend project.

## Notes

- The frontend includes an example call to the backend API (configured via `.env`).
- Copy `.env.example` to `.env` in each project before running it.
