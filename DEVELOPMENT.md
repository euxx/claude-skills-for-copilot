# Development

## Prerequisites

- [Node.js](https://nodejs.org/) 20+
- [VS Code](https://code.visualstudio.com/)

## Setup

1. Clone the repository
2. Install dependencies and set up git hooks:
   ```bash
   npm install
   ```

## Scripts

| Command                | Description                   |
| ---------------------- | ----------------------------- |
| `npm run ci`           | Run all checks (format check) |
| `npm run format`       | Format markdown with Prettier |
| `npm run format:check` | Check markdown formatting     |

## Adding a Skill

1. Create a new directory under `skills/`:
   ```
   skills/<skill-name>/SKILL.md
   ```
2. Write the skill using the [VS Code Agent Skills](https://code.visualstudio.com/docs/copilot/customization/agent-skills) specification.
3. The `name` field in the SKILL.md frontmatter must match the directory name.
4. Register the skill in `package.json` under `contributes.chatSkills`:
   ```json
   { "path": "./skills/<skill-name>/SKILL.md" }
   ```
5. Run `npm run format` to format the new file.

## Project Structure

```
skills/
└── <skill-name>/
    └── SKILL.md    # Skill definition (YAML frontmatter + markdown body)
images/
├── icon.png        # Extension icon (512×512 recommended)
└── icon.svg        # Source vector icon
```

## Installation (local)

Add to VS Code `settings.json`:

```json
"chat.plugins.paths": {
    "/path/to/claude-skills-for-copilot": true
}
```

Or install the packaged VSIX:

```sh
code --install-extension claude-skills-for-copilot-0.0.1.vsix
```
