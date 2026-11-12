# Cryptographic format and key-management requirements

## Security properties
XXTEA provides confidentiality only; it does not authenticate ciphertext. Every encrypted stage MUST therefore have an independent HMAC-SHA-256 authentication tag. The manifest MUST be signed with Ed25519 (or another explicitly versioned asymmetric signature scheme). Android embeds only the public verification key.

## Key separation
Use HKDF-SHA-256 to derive independent encryption and MAC keys from a high-entropy secret held by the trusted publishing/decryption design. Domain-separate by application, catalogue type, schema version, revision, and stage identifier. Never reuse one XXTEA key for HMAC or across stages.

## Critical limitation
A public Android APK that contains a universal symmetric decryption secret can be reverse-engineered. Obfuscation does not solve this. If catalogue access must be restricted, release decryption material through an authenticated HTTPS service after authorization. The currently documented Wispbyte API has no known key-release endpoint; do not assume one exists. Until a secure key-release design exists, do not claim that public encrypted files are inaccessible to APK analysts.

## Envelope
Each stage envelope must carry:
- format identifier and format version;
- catalogue type and schema version;
- revision and stage identifier;
- random nonce/unique per-stage salt where required by the chosen KDF;
- XXTEA ciphertext, encoded as base64url;
- HMAC-SHA-256 tag over the canonical header and ciphertext.

The exact canonical serialization, byte order, padding, XXTEA variant, key size, tag encoding, and test vectors MUST be frozen in a versioned implementation specification before production data is emitted. Do not implement incompatible ad-hoc variants in bot and Android.

## Verification order
1. Verify the signed manifest using the pinned public key.
2. Verify referenced object SHA-256 hashes.
3. Parse bounded envelope headers.
4. Derive stage-specific keys.
5. Verify HMAC in constant time before decryption.
6. Decrypt and validate the stage schema.
7. Assemble the complete record and validate cross-stage invariants.

Reject malformed, oversized, stale, or unsupported data. Never partially activate a catalogue.

## Key handling
Private signing and encryption secrets belong only in a trusted backend or protected secret store. Never commit them to this repository, embed them in the APK, put them in Bot.Business source, or send them over plain HTTP. Rotate keys through an explicit key identifier and signed manifest transition.
