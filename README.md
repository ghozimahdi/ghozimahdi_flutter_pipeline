
# CI/CD Flutter Project 

This Docker image is designed for use in CI/CD pipelines, supporting Flutter.

---

## Usage in GitHub Actions

Below is a sample configuration for using this Docker image in a GitHub Actions workflow:

```yaml
name: CD Dev App - Firebase App Distribution

concurrency:
  group: ${{ github.workflow }}-${{ github.head_ref }}
  cancel-in-progress: true

on:
  push:
    branches: [ "develop" ]
  workflow_dispatch:

jobs:
  build:
    name: Build and Deploy Android App
    runs-on: ubuntu-latest
    timeout-minutes: 120
    container:
      image: mbahgojol/ghozi_fpipe:latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Setup Project
        run: setup
        env:
          encryptKey: ${{ secrets.ENCRYPT_KEY }}
          keystoreKeyAlias: ${{ secrets.KEYSTORE_KEY_ALIAS }}
          keystoreStorePassword: ${{ secrets.STORE_PASSWORD }}
          keystoreKeyPassword: ${{ secrets.KEY_PASSWORD }}
          # setupFile: your setup file 
          # androidWorkingDir: your android dir
          # flutterVersion: 
          # keystoreFile

      - name: Build APK - Flavor Dev Release
        run: build_apk dev
        # env:
          # auditCode: (true/false) running code analyze
          # buildPath: default is "packages/app" for non modular module use "."

      - name: Deploy APK to Firebase
        run: firebase_deploy dev
        env:
          appId: ${{ secrets.ANDROID_FIREBASE_APP_ID }}
          serviceCredentialsFileContent: ${{ secrets.CREDENTIAL_FILE_CONTENT }}
          # groups: "group1, group2"
          # releaseNotes: "release notes"

```

---

## Notes
- This CI/CD configuration works best when using **Ghozi Mahdi CLI** for project generation.
