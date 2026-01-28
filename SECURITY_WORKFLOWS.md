# Checkr Security Workflows

This repository contains reusable GitHub Actions workflows for security scanning.

## Available Workflows

### 1. SAST (sast.yml)
Static code analysis using Semgrep and CodeQL.

### 2. Secret Scanning (secrets.yml)
Detect hardcoded secrets using TruffleHog.

### 3. Dependency Scanning (dependencies.yml + Dependabot)
Scan dependencies for vulnerabilities using Trivy.
Combine with Dependabot for automatic updates (see dependabot-templates/).

### 4. IaC Scanning (iac.yml)
Scan Infrastructure as Code for misconfigurations using Trivy.

### 5. Container Scanning (containers.yml)
Scan Docker images for vulnerabilities using Trivy.

### 6. Complete Security (security-full.yml)
All-in-one workflow that runs all security scans.

## Quick Start

### Option 1: Complete Security Scan (Recommended)

Create `.github/workflows/security.yml` in your repo:

```yaml
name: Security

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  security:
    permissions:
      security-events: write
      contents: read
    uses: checkr/.github/.github/workflows/security-full.yml@main
    with:
      product: 'YourTeamName'  # Set your team/product name
    secrets:
      ARMORCODE_API_KEY: ${{ secrets.CHECKR_GITHUB_SCAN_UPLOAD_ARMORCODE_KEY }}
```

### Option 2: Individual Scans

For more granular control:

```yaml
name: Security

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  sast:
    uses: checkr/.github/.github/workflows/sast.yml@main
    with:
      product: 'YourTeamName'
    secrets:
      ARMORCODE_API_KEY: ${{ secrets.CHECKR_GITHUB_SCAN_UPLOAD_ARMORCODE_KEY }}
    
  secrets:
    uses: checkr/.github/.github/workflows/secrets.yml@main
    with:
      product: 'YourTeamName'
    secrets:
      ARMORCODE_API_KEY: ${{ secrets.CHECKR_GITHUB_SCAN_UPLOAD_ARMORCODE_KEY }}
    
  dependencies:
    permissions:
      security-events: write
      contents: read
    uses: checkr/.github/.github/workflows/dependencies.yml@main
    with:
      product: 'YourTeamName'
    secrets:
      ARMORCODE_API_KEY: ${{ secrets.CHECKR_GITHUB_SCAN_UPLOAD_ARMORCODE_KEY }}
```

**Important:** Jobs that upload to GitHub Security need `security-events: write` permission.

## Recommended: Enable Dependabot Too

For complete dependency security, enable Dependabot alongside Trivy:

1. Go to repo Settings → Security → Enable Dependabot
2. Or copy dependabot-templates/dependabot.yml to your repo

**Why both?**
- Trivy: Scans on PRs and blocks vulnerable dependencies
- Dependabot: Auto-creates PRs to fix vulnerabilities

See dependabot-templates/README.md for details.

## Results

All findings appear in Security tab under Code scanning alerts.

