# FastMAP

> A self-maintaining knowledge map for any codebase — built for Claude Code.

FastMAP gives Claude Code a persistent memory of your project architecture. Instead of scanning directories on every session, Claude reads a single `fastmap.json` file and instantly knows where everything is, what every feature does, and how they connect.

---

## The Problem

Every time you start a Claude Code session on a large project, it wastes time and context scanning folders, reading files, and figuring out the architecture from scratch. Come back after a month and it starts over again.

## The Solution

FastMAP maintains a `fastmap.json` file at the root of your project. Claude Code reads it at the start of every session, updates it when features are added or changed, and uses it to navigate without ever needing a full directory scan.

```
Start session  →  Read fastmap.json  →  Instantly oriented
Add feature    →  Write code         →  Update fastmap.json
Come back      →  Read fastmap.json  →  Pick up exactly where you left off
```

---

## What fastmap.json Looks Like

```json
{
  "project": {
    "name": "MyApp",
    "language": "TypeScript",
    "framework": "Next.js 14",
    "runtime": "Node 20",
    "architecture": "feature-based",
    "database": "PostgreSQL + Prisma",
    "conventions": {
      "components": "PascalCase",
      "files": "kebab-case",
      "tests": "*.spec.ts adjacent to source"
    },
    "entryPoints": {
      "api": "src/api/index.ts",
      "frontend": "src/app/page.tsx"
    }
  },
  "features": {
    "CreateNewUser": {
      "description": "Handles full user registration flow from input to database",
      "path": "src/features/user/create-user.ts",
      "status": "stable",
      "dependencies": ["EmailService", "UserRepository", "AuthService"],
      "methods": {
        "validateInput": "Checks required fields and format (email, password length)",
        "verifyEmail": "Sends confirmation link and waits for token match",
        "hashPassword": "Bcrypt hashes the password before any persistence",
        "saveToDb": "Writes the new user record via UserRepository"
      }
    }
  }
}
```

You can visualize this JSON however you want — build a UI, render a graph, generate docs. The data is yours.

---

## Installation

**1. Download the skill file**

Grab `fastmap.skill` from the [Releases](../../releases) page.

**2. Install in Claude Code**

```bash
claude skills add fastmap.skill
```

**3. That's it.**

FastMAP activates automatically on every Claude Code session.

---

## First Use on an Existing Project

If your project already has code, tell Claude:

```
Index my project and create fastmap.json
```

Claude will scan your project, identify all features and their methods, build the index, and ask you to confirm before saving.

---

## First Use on a New Project

Just start building. Claude will create `fastmap.json` with your project metadata and add features to it as you develop them.

---

## How It Works Day to Day

| What you do | What Claude does |
|---|---|
| Start a session | Reads `fastmap.json`, loads full context |
| Ask "where is the payment logic?" | Looks up index, gives you the exact path |
| Add a new feature | Writes code + adds entry to index |
| Modify a feature | Updates code + updates entry in index |
| Delete a feature | Removes code + removes entry from index |
| Return after a month | Reads index, instantly back in context |

---

## Supported Stacks

FastMAP works with any language or framework. It works especially well with convention-based frameworks because their structure maps cleanly to features:

- **Next.js / React** — pages, components, API routes
- **FastAPI** — routers, services, dependencies
- **Spring Boot** — controllers, services, repositories
- **Laravel** — controllers, services, models
- **Django** — apps, views, models
- **NestJS** — modules, controllers, providers
- **Ruby on Rails** — controllers, models, jobs

See the [`examples/`](./examples) folder for ready-made `fastmap.json` samples for each stack.

---

## Examples

| Stack | File |
|---|---|
| Next.js 14 + TypeScript | [examples/nextjs.json](./examples/nextjs.json) |
| FastAPI + Python | [examples/fastapi.json](./examples/fastapi.json) |
| Spring Boot + Java | [examples/spring-boot.json](./examples/spring-boot.json) |
| Laravel + PHP | [examples/laravel.json](./examples/laravel.json) |

---

## License

MIT
