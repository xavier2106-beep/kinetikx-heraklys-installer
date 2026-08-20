# kinetikx-heraklys-installer

Static site serving Android kiosk APKs + landing pages for HERAKLYS tenants.

**Live:** https://installer.heraklys.com
**Host:** Coolify on `cpanel.csl.staging.heraklys.com` (DO NYC3), auto-deploys on push to `main`.

## Layout

```
public/
├── index.html          # generic tenant list
├── csl/                # Complexe Sportif Longueuil
│   ├── index.html      # bilingual FR/EN landing + QR + install steps
│   ├── manifest.json   # { version, url, sha256 } — polled by kiosk for updates
│   ├── heraklys-kiosk-csl-latest.apk        # stable file the QR / landing button points at
│   └── heraklys-kiosk-csl-<version>.apk     # version-tagged copies (rollback)
├── kinetikx/           # placeholder — future tenant
└── downloads/          # misc downloads
```

## Adding a new APK version (MacAgent flow)

1. Drop `heraklys-kiosk-csl-<new-version>.apk` in `public/csl/`
2. Replace `public/csl/heraklys-kiosk-csl-latest.apk` with the new build
3. Update `public/csl/manifest.json` with the new version + sha256 + release notes
4. Commit + push → Coolify picks up the push, rebuilds container, redeploys
5. Kiosks poll `manifest.json` and auto-update on next check

## Adding a new tenant

1. `mkdir public/<tenant>/`
2. Copy `public/csl/index.html` as a template, edit branding + APK URL
3. Add link in `public/index.html`
4. Commit + push

## Serving details

`nginx.conf`:
- APKs: `Content-Type: application/vnd.android.package-archive`, `Content-Disposition: attachment`, cache 1h
- manifest.json: `no-store`, CORS `*` (kiosks poll from any origin)
- HTML: `no-store`
- Everything else: default nginx
