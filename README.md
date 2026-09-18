# Solana High — SEC September

Static review site, published directly from this repository with GitHub Pages.

## Implemented

A single-spine 3D cover opening, sequential page turns with reduced-motion support, quiet optional page rustle, X-first entry, background preparation states, reveal/download, and an eight-person spread (four per page) with buttons, keyboard arrows, and swipes. Preview contains one supplied Figma portrait and clearly marked empty places; these are not real generated users. Preview sharing downloads the card and opens X for manual attachment.

## Launch dependency — SEC backend

No X credentials, AI generation service, or persistent community database were available. No fake sign-in, generation, posting, or locally persisted community directory is implemented. Configure `authUrl` and `apiBase` in `dist/config.js` only after connecting a real backend. Never put secrets in client files. X OAuth needs a supported server-side authorization path; this static review deliberately does not invent one.

Required HTTPS backend contract (same-origin preferred; if cross-origin, allow the exact site origin with credentials and validate Origin for mutations):

- `authUrl`: top-level X authorization start. Backend handles state/PKCE, callback, secure HttpOnly session, and redirects to site.
- `GET /session`: `{connected: boolean}`; reflects verified X session.
- `POST /portraits` with `{edition}`: authenticated, idempotent per account/edition. Returns full completed record or `{id}` job. Generate from authorized profile inputs server-side, store completed portrait durably, add one community record per verified account. Do not publish incomplete records.
- `GET /portraits/:id`: authorize owner; `{status: 'pending'|'failed'|'complete', id, handle, quote, portraitUrl}`.
- `GET /yearbook?edition=...`: `{entries:[{id,handle,quote,portraitUrl}]}` ordered stably by completion, completed public portraits only. Full edition list currently expected.
- `POST /portraits/:id/share` with `{text}`: authorize owner, upload portrait as X media and create post only on explicit click. Use idempotency per portrait to avoid duplicate posts following uncertain network outcomes. Return `{postUrl}`.

Portrait URLs must be HTTPS, browser-loadable and CORS-readable for download. Serve correctly labelled image content. Protect endpoints with server-side authorization, input validation, CSRF checks, per-user generation limits, and secure token storage. Live mode fails visibly rather than substituting the example portrait.

No backend or persistence is included in this review version. Configure the backend before disabling preview.
