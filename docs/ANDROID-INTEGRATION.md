# Android integration contract

## Network
Use HTTPS raw GitHub URLs from the repository's main branch or, preferably, immutable commit/revision URLs. Apply connection/read timeouts, response size limits, and bounded retries. Never use plain HTTP for credentials or key material.

## Update algorithm
1. Fetch manifest and detached signature.
2. Verify Ed25519 signature against a pinned public key and accepted key ID.
3. Validate manifest schema and revision.
4. Fetch the exact server/tweak objects referenced by the manifest.
5. Verify SHA-256 for each object.
6. Verify each stage HMAC before XXTEA decryption.
7. Decode UTF-8 and validate schema and cross-stage record identity.
8. Atomically persist the complete validated revision.
9. Expose it to UI only after atomic activation.

## Failure behavior
A failed download, signature, hash, HMAC, decryption, or schema check must not erase or replace the last-known-good catalogue. Development diagnostics may log root/debug/emulator/instrumentation indicators, but these checks must not globally block application startup. Do not log credentials, payloads, keys, or decrypted connection records.

## Compatibility
Keep catalogue parsing and cryptography behind a versioned repository/data-source layer. UI screens should consume validated domain models, not parse encrypted blobs directly. Preserve existing VPN engine behavior and existing app navigation while adding catalogue synchronization.

## Missing source requirement
This document is an integration contract, not a claim that Kotlin code has been added. Exact Kotlin integration requires the current Android project source and the actual cryptographic format implementation.
