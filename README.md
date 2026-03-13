# Claude Skills for Copilot

VS Code agent plugin that bundles specialized skills from [anthropics/claude-plugins-official](https://github.com/anthropics/claude-plugins-official) for use with GitHub Copilot.

## Skills

| Skill | Description |
|---|---|
| `/code-review` | Multi-agent code review: bugs, security, AGENTS.md violations |
| `/code-simplifier` | Simplify and refine code for clarity and maintainability |
| `/comment-analyzer` | Detect comment rot and misleading documentation |
| `/feature-dev` | Guided feature development with architecture design and review |
| `/frontend-design` | Create production-grade frontend interfaces with high design quality |
| `/review-toolkit` | Orchestrated end-to-end review (up to 6 specialist sub-skills) |
| `/silent-failure-hunter` | Find silent failures, swallowed errors, and unjustified fallbacks |
| `/test-analyzer` | Behavioral test coverage review — gaps, not just line coverage |
| `/type-design-analyzer` | Type design quality: invariants, encapsulation, enforcement |

## Installation

**Local plugin** — clone the repo and add the path to VS Code `settings.json`:

```json
"chat.plugins.paths": {
    "/path/to/claude-skills-for-copilot": true
}
```

**VS Code extension** — install via VSIX:

```sh
vsce package && code --install-extension claude-skills-for-copilot-0.0.1.vsix
```

## Usage

Type `/` in the Copilot chat input to see all available skills.
