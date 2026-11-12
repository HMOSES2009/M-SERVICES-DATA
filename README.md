# M-SERVICES-DATA

Repository for the M-SERVICES Android application's server and tweak catalogues.

## Planned catalogue files

- `servers.enc` — encrypted server catalogue (XXTEA with per-stage integrity authentication).
- `tweaks.enc` — encrypted tweak catalogue (XXTEA with per-stage integrity authentication).
- `manifest.json` — catalogue versions, hashes, and metadata.
- `manifest.sig` — detached signature for manifest authenticity.

## Security

Never commit GitHub tokens, API keys, passwords, or encryption/signing secrets. Public repository contents must contain ciphertext and public verification metadata only. The Android client must reject invalid catalogue updates and preserve its last-known-good cache.

## Status

Initial repository setup. Encryption format and Android integration must be implemented and tested before publishing production catalogues.
