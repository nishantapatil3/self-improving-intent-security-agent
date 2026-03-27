# Publishing to GitHub Actions Marketplace - Step-by-Step Guide

This guide walks you through publishing the "Publish to Clawhub" action to the GitHub Actions Marketplace.

## Prerequisites

✅ You now have:
- `action.yml` at repository root
- Working action code
- Documentation in `ACTION_README.md`
- All changes committed and pushed

## Step-by-Step Publishing Process

### Step 1: Create a Version Tag

Create a semantic version tag for your release:

```bash
# Create an annotated tag for the release
git tag -a v1.0.0 -m "Release v1.0.0 - Publish to Clawhub GitHub Action"

# Push the tag to GitHub
git push origin v1.0.0
```

**Important**: Use semantic versioning (MAJOR.MINOR.PATCH):
- **MAJOR** (v1.0.0 → v2.0.0): Breaking changes
- **MINOR** (v1.0.0 → v1.1.0): New features, backward compatible
- **PATCH** (v1.0.0 → v1.0.1): Bug fixes

### Step 2: Create Major Version Tag (Recommended)

Create a major version tag so users can reference `@v1`:

```bash
# Create (or force update) the v1 tag
git tag -f v1

# Push it (force push because you'll update this tag)
git push origin v1 --force
```

This allows users to use:
```yaml
uses: nishantapatil3/self-improving-intent-security-agent@v1
```

Instead of:
```yaml
uses: nishantapatil3/self-improving-intent-security-agent@v1.0.0
```

### Step 3: Create GitHub Release

1. **Go to GitHub Releases**:
   - Navigate to: https://github.com/nishantapatil3/self-improving-intent-security-agent/releases
   - Or click "Releases" on the right side of your repo page

2. **Click "Draft a new release"**

3. **Fill in Release Information**:
   - **Choose a tag**: Select `v1.0.0` from dropdown
   - **Release title**: `v1.0.0 - Publish to Clawhub Action`
   - **Description**: Add release notes (see template below)

4. **☑️ IMPORTANT: Check "Publish this Action to the GitHub Marketplace"**
   - This checkbox appears because you have `action.yml` at root
   - If you don't see it, ensure `action.yml` exists and is valid

5. **Select Primary Category**:
   - Choose: **Publishing** (most relevant)
   - Or: **Continuous integration**

6. **Add Secondary Categories** (optional):
   - Deployment
   - Utilities

7. **Review and Publish**:
   - Review the information
   - Click "Publish release"

### Release Notes Template

```markdown
## 🚀 Publish to Clawhub - GitHub Action v1.0.0

A reusable GitHub Action for automatically publishing Claude Code skills and agents to the Clawhub marketplace.

### ✨ Features

- ✅ Automatic publishing to Clawhub on release or manual trigger
- ✅ Validates package.json and openclaw configuration
- ✅ Secure authentication with Clawhub token
- ✅ Optional npm publishing support
- ✅ Detailed publishing summary in workflow output
- ✅ Supports Node.js 20+

### 📖 Usage

\`\`\`yaml
- uses: nishantapatil3/self-improving-intent-security-agent@v1
  with:
    clawhub-token: ${{ secrets.CLAWHUB_TOKEN }}
\`\`\`

### 📚 Documentation

- [Action Documentation](./ACTION_README.md)
- [Example Workflows](./.github/workflows/example-usage.yml)
- [Full Skill Documentation](./SKILL.md)

### 🔧 Requirements

- Node.js >= 20
- Valid `package.json` with `openclaw` configuration
- Clawhub authentication token (get from https://clawhub.ai)

### 🎯 What's Included

This repository serves dual purposes:
1. **Self-Improving Intent Security Agent** - A Claude Code skill
2. **Publish to Clawhub Action** - A reusable GitHub Action

See the README for full details.
```

## Step 4: Verify Publication

After publishing:

1. **Check Marketplace Listing**:
   - Go to: https://github.com/marketplace/actions
   - Search for "Publish to Clawhub" or your action name
   - Or visit directly: https://github.com/marketplace/actions/your-action-name

2. **Verify Badge**:
   - You should see a "Marketplace" badge on your repository
   - Badge location: Next to "About" section

3. **Test the Action**:
   - Create a test repository
   - Add a workflow using your action
   - Verify it works correctly

## Step 5: Add Marketplace Badge to README

Add this badge to your README.md:

```markdown
[![GitHub Marketplace](https://img.shields.io/badge/Marketplace-Publish%20to%20Clawhub-blue?style=for-the-badge&logo=github)](https://github.com/marketplace/actions/publish-to-clawhub)
```

Replace the URL with your actual marketplace URL.

## Updating the Action

When you make improvements:

### For Bug Fixes (Patch Updates)

```bash
# 1. Make your changes and commit
git commit -m "Fix: issue description"
git push

# 2. Create new patch tag
git tag -a v1.0.1 -m "Release v1.0.1 - Bug fixes"
git push origin v1.0.1

# 3. Update major version tag
git tag -f v1
git push origin v1 --force

# 4. Create new GitHub release
```

### For New Features (Minor Updates)

```bash
# 1. Make your changes and commit
git commit -m "Feat: new feature description"
git push

# 2. Create new minor tag
git tag -a v1.1.0 -m "Release v1.1.0 - New features"
git push origin v1.1.0

# 3. Update major version tag
git tag -f v1
git push origin v1 --force

# 4. Create new GitHub release
```

### For Breaking Changes (Major Updates)

```bash
# 1. Make your changes and commit
git commit -m "BREAKING: breaking change description"
git push

# 2. Create new major tag
git tag -a v2.0.0 -m "Release v2.0.0 - Breaking changes"
git push origin v2.0.0

# 3. Create new major version tag
git tag v2
git push origin v2

# 4. Create new GitHub release
# 5. Clearly document breaking changes and migration path
```

## Version Management Best Practices

### Tag Strategy

| Tag | Purpose | Example |
|-----|---------|---------|
| `vX.Y.Z` | Specific release | `v1.0.0`, `v1.0.1` |
| `vX` | Major version (floating) | `v1`, `v2` |
| `vX.Y` | Minor version (floating) | `v1.0`, `v1.1` |

### User References

Users can choose their stability level:

```yaml
# Always get latest v1.x.x (recommended for most users)
uses: youruser/repo@v1

# Always get latest v1.0.x (more conservative)
uses: youruser/repo@v1.0

# Pin to exact version (for critical workflows)
uses: youruser/repo@v1.0.0

# Use main branch (not recommended for production)
uses: youruser/repo@main
```

## Troubleshooting

### "Publish to Marketplace" Checkbox Not Appearing

**Causes**:
1. No `action.yml` at repository root
2. Invalid `action.yml` syntax
3. Repository is private

**Solutions**:
1. Ensure `action.yml` exists at root (not in subdirectory)
2. Validate YAML syntax: https://www.yamllint.com/
3. Make repository public

### Validation Errors

Common issues:
- Missing required fields in `action.yml`: `name`, `description`, `runs`
- Invalid `branding` icon/color
- Syntax errors in YAML

### Action Not Appearing in Search

- Wait 10-15 minutes for indexing
- Check spelling of action name
- Ensure release was published (not draft)
- Verify "Publish to Marketplace" was checked

### Users Report Issues

1. **Check compatibility**: Ensure action works with different OS (ubuntu, windows, macos)
2. **Test thoroughly**: Create test workflows for various scenarios
3. **Document clearly**: Update ACTION_README.md with examples
4. **Version properly**: Use semantic versioning correctly

## Maintenance Checklist

- [ ] Update CHANGELOG.md with each release
- [ ] Keep documentation current (ACTION_README.md)
- [ ] Test on all supported platforms
- [ ] Respond to issues promptly
- [ ] Add examples for common use cases
- [ ] Monitor GitHub Actions logs for errors
- [ ] Keep dependencies updated
- [ ] Watch for security advisories

## Marketing Your Action

1. **Add to README badges**:
   - Marketplace link
   - Version badge
   - Downloads badge

2. **Write a blog post**:
   - Explain problem it solves
   - Show before/after workflow examples
   - Share on dev.to, Medium, etc.

3. **Share on social media**:
   - Twitter with #GitHubActions hashtag
   - LinkedIn
   - Reddit (r/github, r/devops)

4. **Add to awesome lists**:
   - awesome-actions
   - awesome-github-actions

5. **Create video tutorial**:
   - YouTube walkthrough
   - Short demo GIF for README

## Quick Reference Commands

```bash
# Create and publish v1.0.0
git tag -a v1.0.0 -m "Release v1.0.0"
git push origin v1.0.0
git tag -f v1
git push origin v1 --force

# View all tags
git tag -l

# Delete a tag (local and remote)
git tag -d v1.0.0
git push origin :refs/tags/v1.0.0

# View tag details
git show v1.0.0
```

## Next Steps

After publishing:

1. ✅ Create first release (v1.0.0)
2. ✅ Test the action in another repository
3. ✅ Add marketplace badge to README
4. ✅ Share with the community
5. ✅ Monitor for issues and feedback
6. ✅ Plan next features

## Support Resources

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Marketplace Guidelines](https://docs.github.com/en/actions/creating-actions/publishing-actions-in-github-marketplace)
- [Action Metadata Syntax](https://docs.github.com/en/actions/creating-actions/metadata-syntax-for-github-actions)
- [Creating Composite Actions](https://docs.github.com/en/actions/creating-actions/creating-a-composite-action)

---

**Ready to publish?** Follow Step 1 and create your first tag! 🚀
