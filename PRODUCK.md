# Produck setup — unfinished

The Produck React Native SDK was **not** installed. The setup wizard stopped before changing anything. This note is for a developer picking this up fresh.

## What went wrong
- The setup wizard stopped and made no changes.
- Command run (in the repo root):
  ```
  npx @tryproduck/wizard --key pk_live_0123456789abcdef0123456789abcdef0123456789abcdef --yes
  ```
- Its output, quoted exactly:
  > This app uses React Native 0.81.5. The SDK needs React Native 0.82 or newer.
  > Nothing changed.

## Why
The Produck SDK requires **React Native 0.82 or newer**. This app's `package.json` declares `"react-native": "0.81.5"` (with `"expo": "~54.0.32"`). The wizard refuses to proceed on an unsupported React Native version. The automated setup is not allowed to change a package version, so it could not resolve this on its own. No package-manager question was reached — the wizard stopped on the version check first. (The repo is a pnpm project: `packageManager` is `pnpm@10.12.3` and `pnpm-lock.yaml` is present.)

## What this pull request already holds
- No code or dependency changes. The wizard reported "Nothing changed."
- Only this note file.

## What is left
1. Upgrade React Native to 0.82 or newer. In this Expo app that means moving to an Expo SDK whose bundled React Native is >= 0.82, and updating the matching Expo packages. (Could not determine the exact target Expo version from this run — check Expo's SDK release notes.)
2. Re-run the wizard:
   ```
   npx @tryproduck/wizard --key pk_live_0123456789abcdef0123456789abcdef0123456789abcdef --yes
   ```
3. Make each step the wizard then prints. Expected steps from the SDK guide:
   - Install Expo-managed packages:
     ```
     npx expo install react-native-safe-area-context react-native-screens react-native-svg
     ```
   - Install the rest with pnpm:
     ```
     pnpm add @tryproduck/react-native@^2.0.0 react-native-audio-api react-native-get-random-values react-native-incall-manager react-native-webrtc
     ```
   - Add `"@tryproduck/react-native"` to the `plugins` list in `app.config.ts`.
   - Wrap the root component's default export (in `src/app/_layout.tsx`):
     ```tsx
     import { withProduck } from "@tryproduck/react-native";

     export default withProduck(RootLayout, { apiKey: "pk_live_0123456789abcdef0123456789abcdef0123456789abcdef" });
     ```
4. This app currently has **no `ios/` or `android/` folder** (managed Expo), so there is nothing to edit natively by hand and no `pod install` to run yet — Expo applies the config plugin's native changes during prebuild. After prebuild, rebuild with `npx expo run:ios` or `npx expo run:android` to verify the duck appears.

## What to do with this pull request
Either:
- Finish it: upgrade React Native as above, complete the wizard steps, delete this note file, mark the PR ready, and merge; **or**
- Close it, upgrade React Native first, then open a new setup from **Settings › SDK**. A new setup cannot start while this pull request is open.

## Help
Write to yuv@tryproduck.com.
