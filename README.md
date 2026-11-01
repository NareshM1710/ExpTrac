# Ledgerly

A frontend-only, multi-profile personal expense tracker built with HTML, CSS, and vanilla JavaScript.

## Run it

Open `index.html` in a modern browser, or serve the folder locally:

```bash
python -m http.server 8080
```

Then visit `http://localhost:8080`. A local server is recommended because Web Crypto, IndexedDB, and service workers behave most consistently on a secure origin. Chart.js and pdf.js are loaded from cdnjs; all app code is local.

When hosted on GitHub Pages or another HTTPS host, `manifest.json` and `service-worker.js` make the app installable and cache the local app shell for offline startup. Browser extensions and CDN resources may still require a network connection.

## Security model

- Each profile has a random salt and a PBKDF2-SHA-256 password verifier (300,000 iterations). Passwords are never stored.
- The same password-derived, non-extractable AES-GCM `CryptoKey` encrypts the profile's data. Every save uses a fresh random 12-byte IV.
- Profile records and encrypted payloads live in IndexedDB. The 24-hour session stores only the non-extractable `CryptoKey` and an expiry timestamp in a separate IndexedDB store.
- Logging out, locking, or expiry removes the session key. A password change re-encrypts the profile using a newly derived key.
- Backup exports are encrypted JSON envelopes. Restoring one requires the profile password and never uploads data anywhere.

This is client-side privacy, not a substitute for a server-side security boundary. Anyone with access to the unlocked browser profile can use the app, and a compromised browser/device can read data while it is unlocked. Keep encrypted backups somewhere safe; browser storage can be cleared.

## Included features

Monthly-first dashboard, yearly summary, demo data, expenses and income, duplicate-safe recurring entries, repeat-last entry, recent-merchant autocomplete, search and filters, budgets, charts, heatmap, merchant/category insights, splits, encrypted backup/restore, 30-day backup reminders, CSV/PDF-friendly exports, statement CSV import preview, custom categories, currency setting, dark mode, installable PWA shell, keyboard shortcuts (`N` for new entry and `/` for search), and undo after delete.
