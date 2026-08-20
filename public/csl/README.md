# CSL tenant — installer/csl/

Drop `heraklys-kiosk-csl-latest.apk` here + update `manifest.json` with matching version and sha256. Commit + push → Coolify auto-redeploys the static container.

`heraklys-kiosk-csl-latest.apk` is the stable file the landing page + QR point at. Keep a version-tagged copy alongside (e.g. `heraklys-kiosk-csl-0.1.0.apk`) for rollback.
