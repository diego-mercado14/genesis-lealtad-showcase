# Security and verification / Seguridad y verificación

Review date: **2026-11-09**. This report describes the scope of the publication review, not a guarantee that no unknown issue exists.

## Publication boundary

The public repository contains only documentation, an independent static simulator, a fixed non-operational QR and screenshots of that simulator. It contains no product backend, client identity, real customer record, session, database, backup, certificate, `.env` file or private key. The demo makes no fetch/API calls and uses no cookies, analytics or browser storage.

The private product repository includes source, locked dependencies, blank configuration templates, tests and manuals. All local data, generated credentials, certificate material and previous local screenshots are excluded. Public Apple roots are provisioned locally only when Wallet signing is required; they are not committed.

## History review

No pre-existing Git repository or nested history was found in the original project. Both remote repositories were created for this task with one initial README-only commit. That initial content was inspected before uploading source. Publication scans cover the selected files and any local Git history available. Subsequent commits contain only reviewed files; no earlier sensitive history is imported.

## Verification

- Product: 39 automated tests passed with Node 24 on Windows, including the original 37 tests and two additional mobile-gate tests.
- Clean source installation: dependencies installed from the lockfile; tests run without `.env`, certificates, SQLite data or pre-existing credentials.
- Public simulator: reviewed in Chromium at desktop and 390 px mobile width; cancel, simulated stamps, reward, redemption and reset were exercised.
- QR: fixed payload `DEMO-NO-VALIDO`, explicitly not a real card.
- Physical iPhone/Android: camera, browser permissions and background behavior still require device testing.
- Apple Wallet: no genuine signed pass has been validated; APNs updates are not implemented.

The repository publication is separate from application deployment. No production backend or customer database is hosted by this showcase. Operational readiness still depends on real HTTPS hosting, private persistent storage, external backups, monitoring and physical acceptance tests.

Please do not post secrets or personal information in public issues. For professional contact use the maintainer's [GitHub profile](https://github.com/diego-mercado14).
