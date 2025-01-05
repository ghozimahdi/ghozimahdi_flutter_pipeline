
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
        # env:
          # ENCRYPT_KEY: ${{ secrets.ENCRYPT_KEY }} (Optional)
          # KEYSTORE_KEY_ALIAS: ${{ secrets.KEYSTORE_KEY_ALIAS }} (Optional)
          # KEYSTORE_STORE_PASSWORD: ${{ secrets.STORE_PASSWORD }} (Optional)
          # KEYSTORE_KEY_PASSWORD: ${{ secrets.KEY_PASSWORD }} (Optional)
          # SETUP_FILE: your setup file if you have (Optional)
          # ANDROID_WORKING_DIR: your android dir (Optional) (Default value is : packages/app/android)
          # FLUTTER_VERSION: (Optional) (Default value is : 3.27.1
          # KEYSTORE_FILE: (Optional) (Default value is : release/app-keystore.jks)

      - name: Build APK - Flavor Dev Release
        run: build_apk dev (Available values : dev, staging, prod)
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
