# Publish to Clawhub - GitHub Action

A GitHub Action to automatically publish your Claude Code skills and agents to the [Clawhub marketplace](https://clawhub.ai).

## Features

- 🚀 Automatically publish to Clawhub on release or manual trigger
- ✅ Validates package.json and openclaw configuration
- 🔐 Secure authentication with Clawhub token
- 📦 Optional npm publishing support
- 🎯 Composite action (no Docker overhead)
- 📊 Detailed publishing summary in workflow output

## Usage

### Basic Usage

Add this to your `.github/workflows/publish.yml`:

```yaml
name: Publish to Clawhub

on:
  release:
    types: [published]
  workflow_dispatch:

jobs:
  publish:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Publish to Clawhub
        uses: nishantapatil3/self-improving-intent-security-agent@v1
        with:
          clawhub-token: ${{ secrets.CLAWHUB_TOKEN }}
```

### Advanced Usage

```yaml
name: Publish to Clawhub

on:
  release:
    types: [published]
  workflow_dispatch:
    inputs:
      version:
        description: 'Version to publish'
        required: false

jobs:
  publish:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Publish to Clawhub
        uses: nishantapatil3/self-improving-intent-security-agent@v1
        with:
          clawhub-token: ${{ secrets.CLAWHUB_TOKEN }}
          version: ${{ github.event.inputs.version }}
          npm-token: ${{ secrets.NPM_TOKEN }}
          package-dir: './my-skill'
          node-version: '20'
```

## Inputs

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `clawhub-token` | ✅ Yes | - | Clawhub authentication token |
| `version` | ❌ No | From package.json | Version to publish |
| `package-dir` | ❌ No | `.` | Directory containing package.json |
| `npm-token` | ❌ No | - | NPM token for publishing to npm registry |
| `node-version` | ❌ No | `20` | Node.js version (must be >= 20) |

## Outputs

| Output | Description |
|--------|-------------|
| `success` | Whether the publish was successful (`true`/`false`) |
| `package-name` | Name of the published package |
| `package-version` | Version of the published package |
| `clawhub-url` | URL to the skill on Clawhub |

### Using Outputs

```yaml
- name: Publish to Clawhub
  id: publish
  uses: nishantapatil3/self-improving-intent-security-agent@v1
  with:
    clawhub-token: ${{ secrets.CLAWHUB_TOKEN }}

- name: Comment on PR
  if: steps.publish.outputs.success == 'true'
  run: |
    echo "Published ${{ steps.publish.outputs.package-name }}@${{ steps.publish.outputs.package-version }}"
    echo "View at: ${{ steps.publish.outputs.clawhub-url }}"
```

## Setup

### 1. Get Clawhub Token

1. Go to [Clawhub](https://clawhub.ai)
2. Sign in with GitHub
3. Go to Settings → API Tokens
4. Create a new token
5. Copy the token

### 2. Add Token to GitHub Secrets

1. Go to your repository → Settings → Secrets and variables → Actions
2. Click "New repository secret"
3. Name: `CLAWHUB_TOKEN`
4. Value: Paste your Clawhub token
5. Click "Add secret"

### 3. Create Workflow File

Create `.github/workflows/publish.yml` with the usage example above.

### 4. Ensure package.json Has openclaw Config

Your `package.json` must include an `openclaw` configuration:

```json
{
  "name": "my-skill",
  "version": "1.0.0",
  "openclaw": {
    "type": "skill",
    "category": "productivity",
    "tags": ["automation", "helper"]
  }
}
```

## Requirements

- Node.js >= 20 (Clawhub CLI requirement)
- Valid `package.json` with `openclaw` configuration
- Clawhub authentication token

## Publishing This Action to Marketplace

If you want to publish your own fork or use this as a template:

### Step 1: Create a Release

```bash
# Tag the release
git tag -a v1.0.0 -m "Release v1.0.0"
git push origin v1.0.0

# Create a major version tag (recommended)
git tag -f v1
git push origin v1 --force
```

### Step 2: Publish to Marketplace

1. Go to your repository on GitHub
2. Click on "Releases" → "Draft a new release"
3. Select your tag (e.g., `v1.0.0`)
4. Add release title and description
5. ✅ Check "Publish this Action to the GitHub Marketplace"
6. Choose a category (e.g., "Publishing")
7. Click "Publish release"

### Step 3: Verify Publication

1. Your action will appear at: `https://github.com/marketplace/actions/your-action-name`
2. Users can now use it with: `uses: your-username/your-repo@v1`

### Best Practices for Marketplace Actions

1. **Use semantic versioning**: v1.0.0, v1.1.0, etc.
2. **Create major version tags**: `v1`, `v2` (so users can use `@v1`)
3. **Update README**: Clear documentation with examples
4. **Add branding**: Icon and color in `action.yml`
5. **Test thoroughly**: Use in your own workflows first
6. **Maintain changelog**: Document what changed in each version

## Example Repositories Using This Action

- [self-improving-intent-security-agent](https://github.com/nishantapatil3/self-improving-intent-security-agent)

## Troubleshooting

### "Version already exists" Error

This means you're trying to publish a version that's already on Clawhub. Solutions:
1. Bump the version in `package.json`
2. Create a new git tag and release
3. Or specify a different version in the workflow input

### "Not logged in" Error

Your `CLAWHUB_TOKEN` is either:
- Not set in repository secrets
- Invalid or expired
- Not properly referenced in the workflow

### "openclaw config missing" Error

Add an `openclaw` field to your `package.json`:

```json
{
  "openclaw": {
    "type": "skill",
    "category": "your-category"
  }
}
```

## Contributing

Contributions welcome! Please:
1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test with your own skills
5. Submit a pull request

## License

MIT License - see [LICENSE](LICENSE) file for details

## Support

- 🐛 [Report bugs](https://github.com/nishantapatil3/self-improving-intent-security-agent/issues)
- 💡 [Request features](https://github.com/nishantapatil3/self-improving-intent-security-agent/issues)
- 📖 [View documentation](https://nishantapatil3.github.io/self-improving-intent-security-agent/)

## Related

- [Clawhub Marketplace](https://clawhub.ai)
- [Claude Code Documentation](https://github.com/anthropics/claude-code)
- [OpenClaw Specification](https://github.com/openclaw/spec)
