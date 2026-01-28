# Dependabot Configuration Templates

## What is Dependabot?

Dependabot is GitHub's native tool for automatically keeping dependencies up to date and secure.

## How It Works with Our Security Workflows

**Trivy (dependencies.yml)** + **Dependabot** = Complete dependency security

- **Trivy**: Scans on every PR, blocks vulnerable dependencies
- **Dependabot**: Automatically creates PRs to update vulnerable dependencies

## Setup Instructions

### Option 1: Enable via GitHub UI (Easiest)

1. Go to your repository
2. Click **Settings** → **Security** → **Code security and analysis**
3. Enable **Dependabot alerts**
4. Enable **Dependabot security updates**
5. Enable **Dependabot version updates** (optional but recommended)

### Option 2: Add Configuration File (More Control)

1. Copy the template for your language from this directory
2. Create `.github/dependabot.yml` in your repository
3. Customize for your needs
4. Commit and push

**Example:**

```yaml
version: 2
updates:
  - package-ecosystem: "npm"
    directory: "/"
    schedule:
      interval: "weekly"
    open-pull-requests-limit: 10
```

## Configuration Options

### Schedule Intervals
- `daily` - Every day (can be noisy)
- `weekly` - Once per week (recommended)
- `monthly` - Once per month

### PR Limits
- `open-pull-requests-limit: 10` - Max PRs Dependabot will keep open
- Set lower for less noise

### Groups (Reduce PR Noise)
```yaml
groups:
  development-dependencies:
    dependency-type: "development"
  production-dependencies:
    dependency-type: "production"
```

## Best Practices

### ✅ DO:
- Enable Dependabot security updates (auto-fixes critical CVEs)
- Set reasonable PR limits (5-10)
- Add labels for easy filtering
- Group dev dependencies separately
- Review and merge Dependabot PRs regularly

### ❌ DON'T:
- Set interval to "daily" (too noisy)
- Ignore Dependabot PRs for months
- Disable auto-rebase (let Dependabot handle conflicts)

## Language-Specific Templates

### JavaScript/TypeScript
```yaml
- package-ecosystem: "npm"
  directory: "/"
  schedule:
    interval: "weekly"
```

### Python
```yaml
- package-ecosystem: "pip"
  directory: "/"
  schedule:
    interval: "weekly"
```

### Ruby
```yaml
- package-ecosystem: "bundler"
  directory: "/"
  schedule:
    interval: "weekly"
```

### Go
```yaml
- package-ecosystem: "gomod"
  directory: "/"
  schedule:
    interval: "weekly"
```

## Workflow

1. **Dependabot detects vulnerability** (daily scan)
2. **Dependabot creates PR** with fix
3. **Trivy scans the PR** (via dependencies.yml workflow)
4. **Trivy validates the fix** is safe
5. **Team reviews and merges**

## FAQ

**Q: Will Dependabot spam us with PRs?**
A: Set `open-pull-requests-limit` to control this. Start with 10.

**Q: Do we need Dependabot if we have Trivy?**
A: Yes! Trivy detects, Dependabot fixes. They complement each other.

**Q: What if Dependabot's update breaks our code?**
A: Trivy and your CI tests will catch issues before merging.

**Q: Can we customize which dependencies to update?**
A: Yes, use `ignore` rules in dependabot.yml.

## Support

Questions? Check:
- [Dependabot documentation](https://docs.github.com/en/code-security/dependabot)
- Internal security team

