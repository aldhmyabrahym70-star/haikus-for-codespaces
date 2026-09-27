name: Build APK

on:
  push:
    branches: [ main ]
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Setup JDK 17
        uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: 17

      - name: Setup Android SDK
        uses: android-actions/setup-android@v3

      - name: Grant execute to gradlew
        run: chmod +x gradlew

      - name: Build debug APK
        run: ./gradlew assembleDebug --stacktrace

      - name: Upload APK
        uses: actions/upload-artifact@v4
        with:
          name: phishforge-debug
          path: app/build/outputs/apk/debug/app-debug.apk
          retention-days: 30
