# CLAUDE.md

## Releasing

1. Commit all changes
2. `npm version patch` (or `minor`/`major`) — bumps package.json, commits, and creates a git tag
3. `git push && git push --tags` — triggers CI publish workflow
