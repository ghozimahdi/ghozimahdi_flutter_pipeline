
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
          ENCRYPT_KEY: ${{ secrets.ENCRYPT_KEY }}
          KEYSTORE_KEY_ALIAS: ${{ secrets.KEYSTORE_KEY_ALIAS }}
          KEYSTORE_STORE_PASSWORD: ${{ secrets.STORE_PASSWORD }}
          KEYSTORE_KEY_PASSWORD: ${{ secrets.KEY_PASSWORD }}
          # SETUP_FILE: your setup file 
          # ANDROID_WORKING_DIR: your android dir
          # FLUTTER_VERSION: 
          # KEYSTORE_FILE

      - name: Build APK - Flavor Dev Release
        run: build_apk dev
        # env:
          # AUDIT_CODE: (true/false) running code analyze
          # BUILD_PATH: default is "packages/app" for non modular module use "."

      - name: Deploy APK to Firebase
        run: firebase_deploy dev
        env:
          FIREBASE_TOKEN: ${{ secrets.FIREBASE_TOKEN }}
          FIREBASE_APP_ID: ${{ secrets.ANDROID_FIREBASE_APP_ID }}
          # GROUPS: "group1, group2"
          # RELEASE_NOTES: "release notes"

```

---

## Notes
- This CI/CD configuration works best when using **Ghozi Mahdi CLI** for project generation.
