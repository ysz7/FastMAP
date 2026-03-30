---
name: fastmap
version: 1.0.0
description: >
  FastMAP maintains a living JSON index (fastmap.json) that maps every feature and its internal
  methods to their file paths, so Claude Code never has to scan the whole project.
  Use this skill at the START of every session (read the index), when ADDING a feature
  (write to index), when MODIFYING or DELETING a feature (update index), and when SEARCHING
  for a file or understanding architecture (look up index first). Trigger whenever the user
  mentions "feature", "architecture", "where is", "add a feature", "refactor", "delete feature",
  or starts a new Claude Code session on an existing project. Also trigger when the user asks
  what features exist or wants to visualize the project structure. Trigger on any new project
  setup or when the user says "index my project", "map my project", or "use FastMAP".
---

# FastMAP

A self-maintaining knowledge map for any codebase. Claude Code reads and writes
`fastmap.json` to navigate projects instantly — without scanning directories.

---

## Core Protocol

Follow this protocol on every action:

| Situation | Action |
|---|---|
| Session start / returning to project | Read `fastmap.json` → load architecture into context |
| Adding a new feature | Write code → add entry to index |
| Modifying a feature | Change code → update entry in index |
| Deleting a feature | Remove code → remove entry from index |
| Looking for a file | Search index first → then go to the path |
| Onboarding / orientation | Read index → summarize features to user |

**Rule**: Never modify feature code without also updating the index. They must stay in sync.

---

## File Location

Always place the index at the project root:

```
project-root/
└── fastmap.json   ← always here
```

---

## JSON Structure

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
    },
    "AuthenticateUser": {
      "description": "Login flow: validates credentials and issues JWT",
      "path": "src/features/auth/authenticate.ts",
      "status": "stable",
      "dependencies": ["UserRepository", "TokenService"],
      "methods": {
        "findUser": "Looks up account by email in the database",
        "compareHash": "Runs bcrypt compare against stored password hash",
        "issueToken": "Signs and returns a JWT with user role and expiry"
      }
    }
  }
}
```

### Field Reference

**project block** — fill once at project creation, update when stack changes:
- `language` — programming language (e.g. `TypeScript`, `Python`, `Go`)
- `framework` — framework + major version (e.g. `Next.js 14`, `FastAPI 0.110`)
- `runtime` — runtime + major version (e.g. `Node 20`, `Python 3.12`)
- `architecture` — overall pattern (`feature-based`, `layered`, `monorepo`, `microservices`)
- `database` — DB + ORM if any (e.g. `PostgreSQL + Prisma`, `MongoDB + Mongoose`)
- `conventions` — naming rules, test file location, folder patterns
- `entryPoints` — key files to orient a new reader fast

**features block** — one entry per feature, maintained continuously:
- `description` — one sentence: what this feature does for the user
- `path` — relative path to the primary file from project root
- `status` — `stable` | `wip` | `deprecated`
- `dependencies` — other features or services this feature calls
- `methods` — object where each key is a method/function name and value is a short plain-English description of what it does (write this especially when names are non-obvious or abbreviated)

---

## Writing Rules

1. **English only** — all keys, descriptions, and method notes must be in English
2. **Method descriptions are mandatory** when the name is abbreviated, non-obvious, or domain-specific. Skip only when the name is completely self-explanatory (e.g. `getUsers`)
3. **One sentence per description** — be concise, not exhaustive
4. **Paths are relative** to project root, forward-slash separated
5. **Keep status accurate** — mark `wip` while developing, change to `stable` on completion
6. **Dependencies list feature names or service names** — not file paths

---

## Session Start Checklist

When beginning work on a project:

1. Check if `fastmap.json` exists at project root
2. **If yes** → read it, summarize architecture and features to orient yourself
3. **If no** → scan project structure once, create the index, confirm with user before proceeding

---

## Adding a New Feature

After writing the feature code:

```json
"FeatureName": {
  "description": "One sentence describing user-facing purpose",
  "path": "relative/path/to/file.ts",
  "status": "wip",
  "dependencies": [],
  "methods": {
    "methodName": "What this method does in plain English"
  }
}
```

Set status to `wip`. Change to `stable` when done and tested.

---

## Updating a Feature

When modifying existing code, update only the fields that changed:
- New method added → add entry under `methods`
- Method removed → remove its entry
- File moved → update `path`
- New dependency added → add to `dependencies`
- Feature complete → set `status` to `stable`

---

## Deleting a Feature

Remove the entire feature key from the `features` object. Do not leave stubs or comments.

---

## Looking Up a Feature

Before searching the filesystem:

1. Read `fastmap.json`
2. Find the feature by name or by keyword in descriptions
3. Go directly to `path`
4. Check `dependencies` to understand what else is involved

This avoids full directory scans and speeds up navigation significantly.

---

## First-Time Setup on Existing Project

If `fastmap.json` does not exist and the project already has code:

1. Ask the user for: language, framework, runtime, architecture style (if not obvious from files)
2. Scan the project directory structure (up to 2 levels deep)
3. Read key files to identify features, methods, and dependencies
4. Build the full `fastmap.json` — do not skip features, index everything found
5. Write `fastmap.json` to project root
6. Confirm with user: "I've indexed X features. Does this look right?"

For large projects (100+ files), index by directory/module in passes and merge results.

---

## Fresh Project Setup

If starting a brand new project:

1. Ask the user for stack details (language, framework, runtime, database)
2. Create `fastmap.json` with only the `project` block filled in and an empty `features` object
3. Add features to the index as they are created during development
