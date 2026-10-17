# AnshenWin Android — Starter Source

Black + neon-green esports Android app prototype using Kotlin and Jetpack Compose.

## What's included
- Home dashboard and sample tournament cards
- Tournament detail screen with a **demo-only** five-minute countdown
- Room ID/password publication placeholders
- Live lobby toggle concept and HTTPS stream link opener
- Wallet UI and payment/withdrawal setup notices
- POV report form with a 30-minute policy reminder
- Admin UI mock controls
- Firebase Firestore security-rules starting point
- Node.js Cloud Function webhook skeleton with event idempotency and server-only wallet writes

## Important status
This is a runnable **UI starter/prototype**, not a production tournament/payment platform. The current app uses sample data, the timer is local-only, admin controls are not authenticated, no actual video file upload exists, and no payment gateway is connected. Do not accept real entry fees or store real user funds until backend, legal review, security review, provider verification and end-to-end testing are complete.

## Build Android app
1. Install Android Studio (current stable) and JDK 17.
2. Open this `AnshenWin` folder in Android Studio.
3. Allow Gradle sync to download dependencies.
4. Run on an Android 8.0+ emulator/device, or use **Build > Build APK(s)**.

Build configuration: compileSdk 35, minSdk 26, Kotlin 2.0.21, Compose BOM 2024.12.01.

## Production integration checklist
1. Create a Firebase project and configure Android app package `com.anshenwin.app`.
2. Add Firebase Auth; enforce server-side roles and admin custom claims. Remove/guard the in-app Admin tab for normal users.
3. Store player verification results, match state, room credentials and authoritative server timestamps in Firestore. Use Cloud Functions/Cloud Run for eligibility, match start and room credential release. Never trust a device timer.
4. Verify Free Fire account level/rank using an official/authorized method where available; otherwise describe manual review honestly.
5. Add Storage upload for POV files, MIME/size limits, authenticated access and server-side submission-time checks.
6. Select an eligible payment gateway. Replace the demo webhook signature contract with the provider's exact official signature verification, idempotency and reconciliation flow. Configure secrets with Secret Manager; never ship secrets in the APK.
7. Add withdrawal workflow, admin approval, immutable audit records, fraud checks and dispute/appeal process. Do not automatically confiscate wallet balances without lawful, disclosed and reviewed rules.
8. Verify Indian legal requirements and gateway/game publisher policies for paid-entry gaming and payouts before enabling real-money features.

## Webhook skeleton
`functions/src/index.ts` is deliberately provider-neutral. The `x-demo-signature` contract is a placeholder, **not** a ready-to-use Razorpay/Cashfree/PhonePe/Paytm integration. Implement the exact provider docs before deployment.

## Data-model suggestions
- `users/{uid}`: profile, verification status, rank proof metadata, role
- `tournaments/{id}`: mode, entry fee, prize details, status, live lobby config
- `matches/{id}`: room credentials (restricted reads), server timestamps, start state
- `wallets/{uid}`: integer `balancePaise`, server-only writes
- `walletTransactions/{id}`: append-only ledger
- `reports/{id}`: match, reported UID, evidence object path, submittedAt, review status
- `withdrawals/{id}`: amount, UPI token/reference, status, reviewer and audit timestamps
