# Release Guide

## Steps to Release a New Version

1. Update `CHANGELOG.md`:
   - Add version entry: `## [X.Y.Z] - YYYY-MM-DD` with changes

2. Update version in `package.json`:

   ```sh
   # Edit package.json to set "version": "X.Y.Z"
   ```

3. Commit and push:

   ```sh
   git add CHANGELOG.md package.json
   git commit -m "chore: update version to vX.Y.Z"
   git push origin main
   ```

4. Package the extension and create a GitHub release:

   ```sh
   npm run package
   gh release create vX.Y.Z claude-skills-for-copilot-X.Y.Z.vsix \
     --title "vX.Y.Z" \
     --notes "..."
   ```

5. Verify the release was created successfully:

   ```sh
   gh release view vX.Y.Z
   ```

6. Update the release notes on GitHub to match `CHANGELOG.md`:

   ```sh
   gh release edit vX.Y.Z --notes "## What's Changed
   - Change 1
   - Change 2

   **Full Changelog**: https://github.com/euxx/claude-skills-for-copilot/compare/vPREV...vX.Y.Z"
   ```
