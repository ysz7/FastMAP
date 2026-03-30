# Contributing to FastMAP

Thanks for your interest in improving FastMAP!

## What You Can Contribute

- **New examples** — `fastmap.json` samples for frameworks not yet covered (Django, Rails, NestJS, Go Fiber, etc.)
- **Improvements to SKILL.md** — better protocol rules, clearer instructions for edge cases
- **Bug reports** — if FastMAP behaves unexpectedly in Claude Code, open an issue describing what happened

## Adding a New Framework Example

1. Fork the repo
2. Create `examples/your-framework.json` following the structure in existing examples
3. Make sure all descriptions are in English and method descriptions are present for non-obvious names
4. Add a row to the Examples table in `README.md`
5. Open a pull request

## Improving the Skill

1. Fork the repo
2. Edit `SKILL.md`
3. Test by installing the modified skill locally: `claude skills add fastmap.skill`
4. Try a few real sessions with it and note any issues
5. Open a pull request describing what you changed and why

## Ground Rules

- All content in `fastmap.json` files must be in English
- Keep descriptions concise — one sentence per feature or method
- Do not add dependencies or tooling that isn't necessary for the skill itself
