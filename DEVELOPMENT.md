# Development

## Prerequisites

- [Node.js](https://nodejs.org/) 22+
- [VS Code](https://code.visualstudio.com/) with the Extension Development Host

## Setup

1. Clone the repository
2. Install dependencies and set up git hooks:
   ```bash
   npm install
   ```

## Running the Extension

Open the project in VS Code and press **F5** to launch the Extension Development Host. The extension activates automatically on startup.

## Scripts

| Command                | Description                           |
| ---------------------- | ------------------------------------- |
| `npm test`             | Run tests (no-op placeholder)         |
| `npm run ci`           | Run all checks (test + lint + format) |
| `npm run lint`         | Lint (no-op placeholder)              |
| `npm run format`       | Format code (oxfmt)                   |
| `npm run format:check` | Check code formatting                 |
| `npm run package`      | Package extension as `.vsix`          |

## Testing

```bash
npm test     # Run all tests
npm run ci   # Run tests + lint + format check in one step
```

Test and lint scripts are currently no-op placeholders.

## Local Packaging

```bash
npm run package
```

This produces a `.vsix` file. To install it locally:

```bash
code --install-extension my-extension-0.0.1.vsix
```

Or use **Extensions: Install from VSIX** in the VS Code UI.

<!-- END-SHARED -->

## Adding a New Skill

1. Create `skills/<skill-name>/SKILL.md` with the skill content.
2. Register it in `package.json` under `contributes.chatSkills` (keep entries alphabetical):
   ```json
   { "path": "./skills/<skill-name>/SKILL.md" }
   ```
3. Add the skill to the skills table in `README.md` and the per-skill details section under the correct source group.
4. Check the source license: the `LICENSE-APACHE` at the repo root covers both Anthropic and OpenAI sources (Apache 2.0). If adapting from a different source, verify its license is compatible and update the `README.md` License section.
5. Run `npm run ci` to verify formatting.
