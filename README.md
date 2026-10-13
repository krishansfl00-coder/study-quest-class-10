
name: Build Study Quest APK

on:
  workflow_dispatch:

permissions:
  contents: read

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Check out repository
        uses: actions/checkout@v4

      - name: Set up Java
        uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: '17'

      - name: Set up Gradle
        uses: gradle/actions/setup-gradle@v4
        with:
          gradle-version: '8.9'

      - name: Set up Android SDK
        uses: android-actions/setup-android@v3

      - name: Install Android SDK components
        run: sdkmanager "platforms;android-35" "build-tools;35.0.0"

      - name: Extract project and build APK
        run: |
          unzip study_quest_android_project.zip -d project
          cd project
          gradle assembleDebug

      - name: Upload APK
        uses: actions/upload-artifact@v4
        with:
          name: study-quest-debug-apk
          path: project/app/build/outputs/apk/debug/app-debug.apk
          
