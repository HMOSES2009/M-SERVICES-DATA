# Production catalogue publication

This directory is intentionally not populated with placeholder ciphertext.

Production `servers.enc` and `tweaks.enc` can only be generated after the publishing implementation, frozen cryptographic test vectors, protected key custody, and real administrator-approved records are available. Never treat empty files, fabricated records, or plaintext JSON as encrypted production catalogues.

The signed `manifest.json` and `manifest.sig` must be published last as one consistent revision after all referenced encrypted objects have been committed.
