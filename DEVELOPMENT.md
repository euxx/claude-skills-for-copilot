# Development

## Prerequisites

- [Node.js](https://nodejs.org/) 18+
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
| `npm run ci`           | Run all checks (test + lint + format) |
| `npm test`             | Run tests (Vitest)                    |
| `npm run lint`         | Lint code (ESLint)                    |
| `npm run lint:fix`     | Lint and auto-fix                     |
| `npm run format`       | Format code with Prettier             |
| `npm run format:check` | Check code formatting                 |
| `npm run package`      | Package extension as `.vsix`          |

## Testing

```bash
npm test     # Run all tests
npm run ci   # Run tests + lint + format check in one step
```

Tests use [Vitest](https://vitest.dev/).

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
