# Project Conventions

## After Making Changes

After modifying code, run:

- `npm run ci` — tests + lint + format check

See [DEVELOPMENT.md](DEVELOPMENT.md) for all available scripts.

See [RELEASE.md](RELEASE.md) for the release process.

<!-- END-SHARED -->

## Upstream Adaptation Tracking

Skills are adapted from two upstream repos:

- **Anthropic**: `https://github.com/anthropics/claude-plugins-official` — local mirror at `~/projects/claude-plugins-official`
- **OpenAI**: `https://github.com/openai/skills` — local mirror at `~/projects/skills`

Last upstream check: **2026-03-30** (v0.4.0)

- Anthropic checked up to commit `183a6ca`
- OpenAI checked up to commit `736f600`

When checking for updates, compare upstream commits after the "Last upstream check" date above.
