# safe.html

Portable encrypted password manager. Zero dependencies, single file, browser-based.

AES-256-GCM + PBKDF2 (600k iterations) | Web Crypto API | One HTML file = app + data

**Website**: [safehtml.com](https://safehtml.com)

## What is this?

One `safe.html` file is both the application and the vault. Encrypted data is stored inline as base64 between markers inside the HTML. No server, no database, no account — just one file you can put on a USB, email, or store in the cloud.

Open it in any browser. Enter your master password. View, edit, save. Each save downloads a new timestamped file. That's it.

## Quick Start

1. Download `safe.html` from [Releases](../../releases)
2. Open it in Chrome, Edge, Firefox, or Safari
3. Create a new vault with a strong master password
4. Add your entries, click Save, download the encrypted file

## Verify Integrity

Use the included `verify.html` to verify your `safe.html` file hasn't been tampered with:

1. Open `verify.html` in your browser
2. Select your `safe.html` file
3. The tool compares the hash against the known-good build hash

## Source Code

This repository contains the **built distribution files** — the complete, unminified application ready to use. The JavaScript source is fully readable inside `safe.html` for security auditing.

Open `safe.html` in any text editor to inspect the code. What you see is what runs — no hidden dependencies, no external requests, no obfuscation.

## Security

| Property | Detail |
|----------|--------|
| **Cipher** | AES-256-GCM (authenticated encryption) |
| **Key Derivation** | PBKDF2-SHA256, 600,000 iterations, 128-bit random salt |
| **IV** | 96-bit random, fresh on every save |
| **Random** | `crypto.getRandomValues()` (CSPRNG) |
| **CSP** | `default-src 'none'; script-src 'unsafe-inline'; style-src 'unsafe-inline'; form-action 'none'; base-uri 'none'; webrtc 'block'` |
| **Network** | Zero — CSP blocks all outbound requests |
| **Dependencies** | Zero — browser Web Crypto API only |

### App Hardening

- **CSP meta tag**: blocks all outbound network requests, form submissions, and base URI injection
- **Self-hash**: computes SHA-256 of page source, displays in footer for verification
- **verify.html**: companion page for independent file integrity verification

### Data Format

- Encrypted payload: base64-encoded `[16B salt][12B IV][ciphertext+tag]`
- Stored between `<!-- SAFE:BEGIN -->` / `<!-- SAFE:END -->` markers in the HTML
- Each save downloads a new timestamped `safe-YYYYMMDDHHmmss.html` — previous files are natural backups

## Browser Compatibility

safe.html is a local `file://` HTML file:

| Browser | Status | Clipboard |
|---------|--------|-----------|
| **Chrome / Edge** | Full support | Works |
| **Firefox** | Full support | May require permission |
| **Safari** | Full support | Works |

## Files

| File | Purpose |
|------|---------|
| `safe.html` | The password manager — open this to use |
| `verify.html` | Integrity verification tool — verify your safe.html file hasn't been tampered with |

## License

MIT License. Copyright (c) 2026 PING BUSINESS & TECHNOLOGY LIMITED.

See [LICENSE](LICENSE) for full text.
