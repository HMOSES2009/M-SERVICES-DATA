# M-S SERVER BOT publishing contract

## Responsibility
M-S SERVER BOT administers only the M-SERVICES server and tweak catalogues. It must not modify the separate TOTAL NET ACCESS bot or use Wispbyte server/tweak endpoints.

## Required command workflow
- Add, edit, disable, and delete server records.
- Add, edit, disable, and delete tweak records.
- Preview and validate the proposed record before publication.
- Show publication status only after the trusted publisher confirms the GitHub commit.
- Record administrator ID, action, record ID, revision, timestamp, and result in an audit log. Never log passwords, tokens, or plaintext secrets.

## Publication transaction
The bot must call a trusted publishing component over HTTPS. That component validates records, encrypts stages, computes object hashes, signs the manifest, and publishes objects before the manifest. The GitHub token and private cryptographic keys must remain server-side.

Do not put a personal access token in Bot.Business command source, APK source, or public repository files. Do not send secrets to the known HTTP Wispbyte endpoint.

## Conflict handling
If GitHub reports a stale SHA or concurrent update, fetch the latest revision, revalidate, and retry safely. Never overwrite a newer catalogue silently. Return a clear failure to the administrator if publication is incomplete.

## Current integration status
The GitHub repository write path has been tested from this ChatGPT GitHub integration. A production Bot.Business-to-publisher connection has not yet been configured or tested. The existing Wispbyte API does not have a documented catalogue-key delivery route.
