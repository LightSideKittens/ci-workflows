# CI Workflows

Общие workflows для iOS сборки и подписи.

## Структура

```
ci-workflows/
├── .github/workflows/
│   ├── ios-create-certificate.yml  # Один раз на компанию
│   ├── ios-create-profile.yml      # Для каждого приложения (reusable)
│   └── ios-build-ipa.yml           # Сборка IPA (reusable)
└── fastlane/
    ├── Fastfile
    ├── Matchfile
    ├── Appfile
    └── Gemfile
```

## Порядок настройки

### 1. Один раз на компанию

Запустить в этом репозитории: **Actions → iOS Create Certificate → Run workflow**

Создаст Distribution Certificate и сохранит в `ios-certificates`.

### 2. Для каждого приложения

В репозитории приложения создать `.github/workflows/setup-ios.yml`:

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

Запустить: **Actions → Setup iOS → Run workflow**

## Required Secrets

### Organization level:
- `GH_PAT`
- `MATCH_PASSWORD`
- `MATCH_GIT_URL`
- `TEAM_ID`
- `APP_STORE_CONNECT_KEY_ID`
- `APP_STORE_CONNECT_ISSUER_ID`
- `APP_STORE_CONNECT_API_KEY`

### Repository level (для каждого приложения):
- `APP_IDENTIFIER` — Bundle ID приложения