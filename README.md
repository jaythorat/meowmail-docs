# MeowMail developer docs (`docs.meowmail.in`)

Mintlify-hosted documentation for the public API at `/api/v1`. Deployed by
Mintlify from this folder on push; there is no build step in our own pipeline and
nothing here ships in the frontend bundle.

## Layout

| Path | What it is |
|---|---|
| `docs.json` | Site config: theme, colors, navigation, OpenAPI source |
| `*.mdx` | The guide. One page per concept, matching `navigation` in `docs.json` |
| `logo/`, `favicon.svg` | Brand assets, copied from `frontend/public/` |

## The API reference is not written by hand

`docs.json` points the reference tab at the **live spec URL**:

```json
"openapi": "https://api.meowmail.in/api/v1/openapi.json"
```

Not a committed copy, and not a relative path to `priv/openapi.json`. This is
deliberate: it makes drift structurally impossible, because the reference is
always generated from exactly what the API is serving. The previous
self-hosted setup kept a second copy of the spec and it drifted within a day.

**Consequence for ordering:** a spec change appears in the docs only after the
backend is deployed. Deploy the API first, then push docs. If Mintlify builds
while the API is unreachable, the build fails rather than publishing a reference
for endpoints that do not exist — which is the behaviour we want.

## Editing

Prose lives here as MDX. Endpoint schemas live in `priv/openapi.json` at the repo
root — edit them there, never here.

Local preview:

```bash
npm i -g mint     # Mintlify CLI
cd docs-site
mint dev          # http://localhost:3000
```

## Custom domain

`docs.meowmail.in` needs a CNAME record pointing at the target Mintlify shows in
its dashboard, plus the domain registered there. On Cloudflare the record must be
DNS-only (grey cloud), not proxied. See `docs/DEPLOYMENT_GUIDE.md`.

## Why Mintlify

Recorded in `docs/PUBLIC_API_PLAN.md` §7, including what was tried first
(self-hosted Scalar on the apex) and why it was removed.
