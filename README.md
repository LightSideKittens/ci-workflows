# CI Workflows

Shared workflows for iOS build and signing.

## Structure

```
ci-workflows/
├── .github/workflows/
│   └── ios-create-profile.yml      # Per-app provisioning (reusable)
└── fastlane/
    ├── Fastfile
    ├── Matchfile
    ├── Appfile
    └── Gemfile
```

## Setup

### Per application

Create `.github/workflows/setup-ios.yml` in the app repository:

```yaml
name: Setup iOS

on:
  workflow_dispatch:
    inputs:
      profile_type:
        type: choice
        options: [adhoc, appstore]
        default: adhoc

jobs:
  setup:
    uses: LightSideKittens/ci-workflows/.github/workflows/ios-create-profile.yml@main
    with:
      profile_type: ${{ inputs.profile_type }}
    secrets: inherit
```

Run: **Actions → Setup iOS → Run workflow**

## Required Secrets

### Organization level:
- `GH_PAT`
- `MATCH_PASSWORD`
- `MATCH_GIT_URL`
- `TEAM_ID`
- `APP_STORE_CONNECT_KEY_ID`
- `APP_STORE_CONNECT_ISSUER_ID`
- `APP_STORE_CONNECT_API_KEY`

### Repository level (per app):
- `APP_IDENTIFIER` — App Bundle ID

## How It Works

**Certificates** (Distribution Certificate):
- Automatically created on first `Setup iOS` run for any app
- Shared across the organization (Team ID)
- Reused for all subsequent apps
- Stored in the `ios-certificates` repository

**Profiles** (Provisioning Profile):
- Created separately for each app (Bundle ID)
- Linked to a specific certificate
- Stored in the `ios-certificates` repository
