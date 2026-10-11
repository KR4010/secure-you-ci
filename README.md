# Secure You CI

The application is private at KR4010/secure-you. This public repository contains only the workflow. Like Cambium CI, iOS compilation uses `eas build --local` on a GitHub macOS runner. EAS manages signing credentials and the TestFlight submission. No application source or IPA is uploaded as a public Actions artifact.

Checks run on Linux first: TypeScript, lint, domain tests, and an iOS Metro bundle. The build job checks out the exact commit that passed the gate. A requested revision must belong to the private app's main branch history. Automatic dispatches accept only KR4010 as sender. Never add pull_request_target or untrusted PR execution to this workflow.

## Secrets

- APP_REPO_SSH_KEY: read-only deploy key for KR4010/secure-you.
- EXPO_TOKEN: Expo access token for the Secure You project owner.
- EXPO_TEAM_ID: Apple Developer team ID.
- ASC_APP_ID: Secure You's own numeric App Store Connect app ID (never Cambium's).
- ASC_API_KEY_P8_BASE64: base64 App Store Connect private API key.
- ASC_KEY_ID and ASC_ISSUER_ID: API key identifiers.

Apple signing certificates and the Secure You provisioning profile must be configured with `eas credentials:configure-build -p ios -e testflight` once before unattended builds. The SQLCipher build uses encryption; resolve Apple's export-compliance requirements for the app before testers can install it.

## Run

Actions → Secure You CI / TestFlight → Run workflow. Select main or a full source SHA. Turn build off for checks only. Turn submit off to compile without uploading. Pushes to private main dispatch an iOS build and TestFlight submission via CI_DISPATCH_TOKEN stored only in the private repo.

Submissions use the exact IPA produced in the job. EAS submission completion establishes upload, not tester availability. Inspect App Store Connect processing, encryption compliance, and internal group assignment separately. Public logs must never include journal fixtures, keys, `.env`, or application source dumps.
