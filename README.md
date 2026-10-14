# EduFace - static HTML edition

Face-recognition attendance and visitor management as **plain HTML, CSS and JavaScript**. No server code, no database server, no installation: data lives in the browser (IndexedDB) and the app runs from a folder.

The folders are **cumulative**: each phase is a complete, runnable copy containing everything from the earlier phases plus its own additions.

| Folder | Adds | Size |
|---|---|---|
| `phase1/` | First-time setup, sign-in with roles, lockout, idle sign-out, users, tamper-evident audit log, settings, languages, dark mode | 0.1 MB |
| `phase2/` | + Dashboard, classes, students and guardians, CSV import/export, printable QR cards | 0.1 MB |
| `phase3/` | + Attendance (QR by camera/USB scanner/typed), late cut-off, manual override with reason, close day, visitors and badges, roll call, reports (CSV/print) | 0.4 MB |
| `phase4/` | + Guardian consent, face enrolment, live face recognition (webcam), encrypted face templates, consent withdrawal deletes faces | 8 MB |
| `phase5/` | + Gate kiosk (PIN), encrypted backup/restore, offline installable app, guardian message queue, data retention | 8 MB |
| `phase6/` | + Built-in help (training guide, privacy checklist, accuracy plan), demo data, self-test page | 8 MB |

## Run any phase
1. Open the phase folder and double-click **`start.bat`** (PowerShell must be allowed to run; nothing is installed).
2. It starts a tiny web server on this computer only and opens `http://localhost:8080`.
3. First time: create the school and administrator.

Use Chrome or Edge. The address must be `localhost` for the camera to work.

## Important limits
- **All data stays in one browser on one computer.** Several gates/tablets cannot share live data. Clearing browser data deletes everything: use **Backup** (phase 5+).
- Each phase folder includes db/: the MySQL 8 schema, **not used by this browser edition** (it is the database design for a server-backed version).
- No server means **no automatic SMS** (messages are queued; one tap opens the SMS app) and **no multi-device sync**.
- **No anti-spoofing** (a printed photo or phone screen can fool the face scan).
- **Face accuracy has not been measured** on your students/camera. The engine here (face-api, small models) is lighter than the server edition's. See `phase6` -> Help -> Accuracy test plan.
- Security is enforced in the browser: protect the computer (Windows login, BitLocker, locked room).
- On an older computer the **first** face-engine start can pause the screen for up to a minute (one-time GPU compile); later starts take about 5 seconds.

## Check it works
Open `phase6/selftest.html` (via `start.bat`, then click *Run self-test* in Help). It runs 24 checks against a throw-away database.

## For developers
```
node _tools/build-phases.js     # rebuild phase1..phase6 from _src/ and _vendor/ (syntax-checks sources first)
node _tools/check.js            # verify every phase references existing, parseable scripts
```
- `_src/core` shell, database (IndexedDB), crypto (WebCrypto), audit chain, auth
- `_src/modules` one file per feature; phases are lists of modules (`_tools/build-phases.js`)
- `_vendor` third-party files (checksums in `_vendor/SHA256SUMS.txt`): face-api 1.7.15 (MIT), jsQR 1.4.0 (Apache-2.0), qrcode-generator 2.0.4 (MIT)

