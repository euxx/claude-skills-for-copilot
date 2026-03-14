# Release Guide

## Steps to Release a New Version

1. Update `CHANGELOG.md`:
   - Add version entry: `## [X.Y.Z] - YYYY-MM-DD` with changes

2. Update version in `package.json`:

   ```bash
   # Edit package.json to set "version": "X.Y.Z"
   npm install  # sync package-lock.json
   ```

3. Commit and push:

   ```bash
   git add CHANGELOG.md package.json package-lock.json
   git commit -m "chore: update version to vX.Y.Z"
   git push origin main
   ```

4. Run the release workflow:

   ```bash
   gh workflow run release.yml
   ```

   This runs tests, packages the extension as `.vsix`, and creates a GitHub Release with the file attached.

5. Verify the release:

   ```bash
   gh release view vX.Y.Z
   ```

<!-- END-SHARED -->

6. Update the release notes on GitHub to match `CHANGELOG.md`:

   ```bash
   gh release edit vX.Y.Z --notes "## What's Changed
   - Change 1
   - Change 2

   **Full Changelog**: https://github.com/euxx/claude-skills-for-copilot/compare/vPREV...vX.Y.Z"
   ```
