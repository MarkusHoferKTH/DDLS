# Agent instructions

## Version control

Before any big change—installing packages, rewriting a working file, or undertaking a large refactor—commit the current state first. Commit again whenever something starts working. Use short, clear commit messages so there is always history to roll back to.

## Course fold service

To fold a protein sequence that isn't in the AlphaFold DB, read https://ddls-structure-api-8a7d6803.svc.hypha.aicell.io/skill.md and follow it. Load the fold key from the `DDLS_FOLD_KEY` variable in `.env` with `set -a; source .env; set +a`, then send it as the Bearer token. Never write the key itself into `AGENTS.md` or any other committed file.
