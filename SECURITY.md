# Security policy

## Never publish
- GitHub tokens, bot tokens, API keys, passwords, private signing keys, or symmetric catalogue secrets.
- Plaintext production server credentials or decrypted catalogue exports.
- Logs containing payloads, UUIDs, usernames, passwords, or key material.

## Reporting
Do not open public issues containing credentials or exploitable details. Rotate any secret that has been exposed. Report security concerns privately to the repository owner.

## Integrity
Production catalogue releases must be signed. XXTEA ciphertext must also be authenticated with HMAC-SHA-256. A SHA-256 hash alone detects accidental changes only when the manifest itself is trusted; it is not a substitute for a signature.
