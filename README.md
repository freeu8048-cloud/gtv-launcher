name: Build APK
on:
  workflow_dispatch:
  push:
    paths:
      - 'GTVLauncher.zip'
permissions:
  contents: write
jobs:
  build:
    runs-on: ubuntu-24.04
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: 17
      - run: yes | sdkmanager --licenses || true
      - uses: gradle/actions/setup-gradle@v4
        with:
          gradle-version: "8.7"
      - run: unzip -q GTVLauncher.zip
      - run: gradle assembleDebug
        working-directory: GTVLauncher
      - uses: softprops/action-gh-release@v2
        with:
          tag_name: latest
          name: Latest build
          files: GTVLauncher/app/build/outputs/apk/debug/app-debug.apk
