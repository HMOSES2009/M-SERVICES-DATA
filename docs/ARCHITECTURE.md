# M-SERVICES catalogue architecture

## Source of truth
GitHub repository `HMOSES2009/M-SERVICES-DATA`, branch `main`, is the authoritative distribution source for server and tweak catalogues. Android reads public raw files over HTTPS. The M-S SERVER BOT is the administration interface and publishes validated catalogue revisions to GitHub.

Wispbyte remains reserved for formation access and user authorization. It is not the server/tweak catalogue store.

## Data flow
1. An authorized administrator submits a server or tweak through M-S SERVER BOT.
2. The bot validates required fields, types, lengths, and identifiers.
3. A trusted publishing component encrypts each stage, authenticates it, updates the manifest, signs the manifest, and commits a consistent revision.
4. Android downloads the manifest and signature, verifies the signature, downloads the referenced ciphertext, verifies hashes and per-stage authentication, then decrypts.
5. Android validates the decoded schema before replacing its last-known-good local catalogue.

## Catalogue staging
Servers use four independently authenticated encrypted stages:
1. Display identity: server name and country.
2. Proxy endpoint: proxy host.
3. Payload: payload string.
4. Connection parameters: port, SNI, DNS, UUID, protocol, and other supported connection fields.

Tweaks use two independently authenticated encrypted stages:
1. Display identity: tweak name, country, and operator.
2. Configuration: host, payload, SNI, DNS, and other supported parameters.

Stage boundaries and field names are versioned. A client must reject unsupported schema versions rather than guessing.

## Consistency and rollback
A manifest identifies one complete catalogue revision. Publish data objects first and the signed manifest last. Android only activates a revision after every referenced object passes signature, hash, authentication, decryption, and schema validation. On any failure, retain the previous known-good revision.

## Non-goals
This repository does not contain GitHub credentials, bot tokens, private signing keys, universal decryption secrets, or plaintext production credentials.
