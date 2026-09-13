# Rest Hours

Two Android apps for checking seafarers' work and rest hours against STCW and MLC limits.

- **Rest Hours** (`apps/crew`) — each crew member fills in their own day on a half-hour grid and sees immediately whether they are compliant.
- **Rest Hours Master** (`apps/master`) — collects those records by QR code and reviews them.

Everything runs offline. No server, no account, no data leaves the phone.

## The four rules

| | Requirement | Reference |
|---|---|---|
| R1 | At least 10 h rest in every rolling 24 h | STCW Code A-VIII/1.2.1 · MLC Std A2.3.5(b)(i) |
| R2 | At least 77 h rest in every rolling 7 days | STCW Code A-VIII/1.2.2 · MLC Std A2.3.5(b)(ii) |
| R3 | Rest in no more than 2 periods, one of at least 6 h | STCW Code A-VIII/1.3 · MLC Std A2.3.6 |
| R4 | No more than 14 h between consecutive rest periods | STCW Code A-VIII/1.3 · MLC Std A2.3.6 |

Rule 3 counts a rest period once, at the rolling window in which it begins, and measures its
true unbroken length. This is what makes 4-on/8-off and 6-on/6-off read correctly as two rest
periods rather than three when a period crosses midnight.

These apps are a **planning aid**. They sit alongside the official signed record of hours, they
do not replace it. Making them the record itself brings in MLC Std A2.3.12 — seafarer signature,
master endorsement, a copy issued to the seafarer, retention — and probably flag state approval.

## Building the APKs

Push this repository to GitHub. The workflow in `.github/workflows/build-apk.yml` runs on every
push to `main` and can also be started by hand from the **Actions** tab.

When it finishes, open the run and download the two artifacts:

- `rest-hours-crew-apk`
- `rest-hours-master-apk`

Each contains an installable `.apk`. Nothing needs to be installed on your own machine.

The APKs are debug-signed. They install and share fine by WhatsApp, Bluetooth or SD card.
Publishing to the Play Store would need a release keystore, which is deliberately not included
here — no signing secrets live in this repository.

To change the app name or package id, edit `apps/<app>/app.json`.

## Hosting instead of installing

`apps/crew/www` and `apps/master/www` are complete progressive web apps — manifest, icons and a
service worker. Serve either folder over HTTPS and Chrome will offer **Install app**, with the
same offline behaviour as the APK. Useful if you would rather send a link than a file.

The camera only works on HTTPS or inside the APK. From a plain downloaded file it is blocked by
the browser, so use the photo or paste routes there.

## Moving records between phones

The crew app's **Backup** screen produces the whole record as a short text string — about two
weeks fits in 135 characters. Paste it into the **Restore** box on another phone to bring
everything across, including the name and rank. There is a restore link on the first-run screen
for a crew member starting on a new handset.

The master app exports every crew member, every day, all four rule outcomes and your written
instructions as CSV.
