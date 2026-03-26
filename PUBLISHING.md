# Publishing Guide

This document explains how to publish new versions of this skill to Clawhub.

## Automated Publishing (Recommended)

The repository includes a GitHub Actions workflow that automatically publishes to Clawhub when you create a new release.

### Quick Release

```bash
# 1. Update version
npm version patch  # or minor, major

# 2. Push with tags
git push --follow-tags

# 3. Create GitHub release
gh release create v$(node -p "require('./package.json').version") \
  --title "Release v$(node -p "require('./package.json').version")" \
  --generate-notes
```

The GitHub Action (`.github/workflows/publish.yml`) will automatically:
- ✅ Validate package.json and openclaw configuration
- ✅ Run tests
- ✅ Publish to npm (if `NPM_TOKEN` secret is configured)
- ✅ Notify Clawhub (if webhook configured)

### Setup GitHub Secrets

Configure these secrets in your repository settings (`Settings` > `Secrets and variables` > `Actions`):

#### Option 1: npm Publishing (if Clawhub syncs from npm)

```
NPM_TOKEN=npm_xxxxxxxxxxxx
```

To get an npm token:
1. Create account at https://npmjs.com
2. Go to Account > Access Tokens
3. Generate new "Automation" token
4. Add to GitHub secrets

#### Option 2: Clawhub Direct Publishing

**Required secret:**
```
CLAWHUB_TOKEN=clawhub_xxxxxxxxxxxx
```

**Optional secret** (only if you know the specific endpoint):
```
CLAWHUB_WEBHOOK_URL=https://clawhub.ai/api/webhook/publish
```

The workflow will:
1. First try `npx skills publish` with your CLAWHUB_TOKEN (recommended)
2. If that fails, automatically try common webhook endpoints:
   - `https://clawhub.ai/api/webhook/publish`
   - `https://clawhub.ai/api/v1/publish`
   - `https://api.clawhub.ai/webhook/github`
3. Use custom CLAWHUB_WEBHOOK_URL if you've provided it

**You only need CLAWHUB_TOKEN** - the webhook URL is optional and will be auto-detected!

## Manual Publishing

### Method 1: Using npm

If Clawhub syncs from npm registry:

```bash
# Login to npm
npm login

# Publish
npm publish --access public
```

### Method 2: Using Clawhub CLI

If Clawhub provides a dedicated CLI:

```bash
# Install CLI (check Clawhub docs for exact package name)
npm install -g @clawhub/cli

# Login
clawhub login

# Publish
clawhub publish
# or
npx skills publish
```

### Method 3: GitHub Integration

Some package registries auto-sync from GitHub:

1. Ensure `package.json` has correct repository URL
2. Create a GitHub release with proper version tag
3. Clawhub automatically detects and syncs

This is likely how Clawhub works based on its GitHub integration features.

## Version Management

### Semantic Versioning

Follow [semver](https://semver.org/) principles:

- **Patch** (1.0.0 → 1.0.1): Bug fixes, documentation updates
  ```bash
  npm version patch
  ```

- **Minor** (1.0.0 → 1.1.0): New features, backward compatible
  ```bash
  npm version minor
  ```

- **Major** (1.0.0 → 2.0.0): Breaking changes
  ```bash
  npm version major
  ```

### Pre-release Versions

For testing before official release:

```bash
# Create alpha version
npm version prerelease --preid=alpha
# 1.0.0 → 1.0.1-alpha.0

# Create beta version
npm version prerelease --preid=beta
# 1.0.1-alpha.0 → 1.0.1-beta.0
```

## Pre-publish Checklist

Before publishing a new version:

- [ ] All tests pass: `npm test`
- [ ] Documentation is updated:
  - [ ] README.md
  - [ ] SKILL.md
  - [ ] CLAUDE.md
  - [ ] CHANGELOG.md (if you maintain one)
- [ ] Version number is appropriate (patch/minor/major)
- [ ] `package.json` metadata is correct:
  - [ ] Repository URLs
  - [ ] Author information
  - [ ] Keywords
  - [ ] openclaw configuration
- [ ] Examples are working
- [ ] Scripts are tested:
  - [ ] `./scripts/setup.sh`
  - [ ] `./scripts/validate-intent.sh`
  - [ ] `./scripts/report.sh`

## Verifying Publication

After publishing, verify:

1. **Clawhub Page**: Visit https://clawhub.ai/nishantapatil3/self-improving-intent-security-agent
   - [ ] New version appears
   - [ ] GitHub link is visible
   - [ ] Documentation link works

2. **Installation**: Test the skill can be installed
   ```bash
   npx skills add nishantapatil3/self-improving-intent-security-agent
   ```

3. **GitHub Release**: Check https://github.com/nishantapatil3/self-improving-intent-security-agent/releases
   - [ ] Release notes are clear
   - [ ] Tags match version

## Troubleshooting

### GitHub Action Fails

Check the workflow run at:
https://github.com/nishantapatil3/self-improving-intent-security-agent/actions

Common issues:
- Missing secrets (NPM_TOKEN, CLAWHUB_TOKEN)
- Invalid package.json
- Failed tests
- Permission issues

### Clawhub Not Syncing

If Clawhub doesn't show new version:

1. **Check sync delay**: May take a few minutes to hours
2. **Verify package.json**: Ensure repository URL is correct
3. **Check GitHub release**: Must be a proper release, not just a tag
4. **Contact Clawhub support**: They may need to manually trigger sync

### npm Publish Errors

```bash
# Check if package name is available
npm view self-improving-intent-security-agent

# Check if you're logged in
npm whoami

# Verify package.json
npm pack --dry-run
```

## Rollback

If you need to unpublish or rollback:

### npm
```bash
# Deprecate a version (recommended over unpublish)
npm deprecate self-improving-intent-security-agent@1.0.1 "Version deprecated, use 1.0.2"

# Unpublish (only within 72 hours)
npm unpublish self-improving-intent-security-agent@1.0.1
```

### Clawhub
Check Clawhub documentation for version management and deprecation.

## Automation Tips

### Automatic Version Bumping

Add to `.github/workflows/publish.yml` to auto-increment versions:

```yaml
- name: Bump version
  run: npm version patch --no-git-tag-version
```

### Release Drafter

Use [Release Drafter](https://github.com/release-drafter/release-drafter) to automatically generate release notes from PRs.

### Conventional Commits

Use [conventional commits](https://www.conventionalcommits.org/) to automatically determine version bumps:

```bash
# Install
npm install -g standard-version

# Create release
standard-version

# Push
git push --follow-tags
```

## Support

For publishing issues:
- **GitHub Issues**: https://github.com/nishantapatil3/self-improving-intent-security-agent/issues
- **npm Support**: https://npmjs.com/support
- **Clawhub Support**: Check Clawhub documentation/contact

---

**Next Steps**: Set up your `NPM_TOKEN` or `CLAWHUB_TOKEN` secret in GitHub to enable automated publishing!
