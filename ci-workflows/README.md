# CI Workflows

Общие reusable workflows для iOS сборки и подписи.

## Структура

```
ci-workflows/
├── .github/workflows/
│   ├── ios-setup-certificates.yml  # Создание сертификатов (один раз на приложение)
│   └── ios-build-ipa.yml           # Сборка подписанного IPA
└── fastlane/
    ├── Fastfile    # Lanes для Match и сборки
    ├── Matchfile   # Конфигурация Match
    ├── Appfile     # App ID и Team ID
    └── Gemfile     # Ruby зависимости
```

## Использование

### 1. Создание сертификатов (один раз для нового приложения)

В репозитории приложения создать `.github/workflows/setup-ios.yml`:

```yaml
name: Setup iOS Certificates

on:
  workflow_dispatch:

jobs:
  setup:
    uses: LightSideKittens/ci-workflows/.github/workflows/ios-setup-certificates.yml@main
    secrets: inherit
```

### 2. Сборка IPA

```yaml
jobs:
  build-ios:
    uses: LightSideKittens/ci-workflows/.github/workflows/ios-build-ipa.yml@main
    with:
      xcode_project_path: build/iOS/Unity-iPhone.xcodeproj
      output_name: MyApp.ipa
    secrets: inherit
```

## Required Secrets

### Organization level:
- `GH_PAT` - GitHub Personal Access Token
- `MATCH_PASSWORD` - Пароль шифрования сертификатов
- `MATCH_GIT_URL` - URL репозитория сертификатов
- `TEAM_ID` - Apple Team ID
- `APP_STORE_CONNECT_KEY_ID` - App Store Connect API Key ID
- `APP_STORE_CONNECT_ISSUER_ID` - App Store Connect Issuer ID
- `APP_STORE_CONNECT_API_KEY` - Содержимое .p8 файла

### Repository level:
- `APP_IDENTIFIER` - Bundle ID приложения