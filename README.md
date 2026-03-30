# FastMAP

Persistent knowledge map for Claude Code. Claude reads `fastmap.json` once per session and instantly knows your project structure — no directory scanning.

## Installation

**1. Download** [`fastmap.skill`](https://github.com/ysz7/FastMAP/raw/main/fastmap.skill)

**2. Install**

```bash
claude skills add fastmap.skill
```

Done. FastMAP activates automatically on every session.

---

## Usage

**New project** — just start building. Claude creates and maintains `fastmap.json` automatically.

**Existing project** — tell Claude:

```
Index my project and create fastmap.json
```

---

## How It Works

| Action | What Claude does |
|---|---|
| Session start | Reads `fastmap.json`, loads full context |
| Add a feature | Writes code + adds entry to index |
| Modify a feature | Changes code + updates entry in index |
| "Where is the payment logic?" | Looks up index, returns exact path |
| Return after a month | Reads index, instantly back in context |

---

## fastmap.json Structure

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
        "hashPassword": "Bcrypt hashes the password before any persistence",
        "saveToDb": "Writes the new user record via UserRepository"
      }
    }
  }
}
```

---

## Examples

| Stack | File |
|---|---|
| Next.js 14 + TypeScript | [nextjs.json](./nextjs.json) |
| FastAPI + Python | [fastapi.json](./fastapi.json) |
| Spring Boot + Java | [spring-boot.json](./spring-boot.json) |
| Laravel + PHP | [laravel.json](./laravel.json) |

---

## License

MIT
