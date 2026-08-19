# MeowMail developer docs

Source for **[docs.meowmail.in](https://docs.meowmail.in)** — the developer
documentation for the free MeowMail temporary-email API.

Built and hosted by [Mintlify](https://mintlify.com), deployed automatically on
push to `main`. This repo is public so Mintlify and doc contributors can read it;
the service itself lives in a separate private repo.

## Layout

| Path | What it is |
|---|---|
| `docs.json` | Site config: theme, colors, navigation, OpenAPI source |
| `*.mdx` | The guide. One page per concept, matching `navigation` in `docs.json` |
| `logo/`, `favicon.svg` | Brand assets |

## The API reference is not written by hand

`docs.json` points the reference tab at the **live spec URL**:

```json
"openapi": "https://api.meowmail.in/api/v1/openapi.json"
```

Not a committed copy of the spec, and not a path into another repo. This is the
reason a separate docs repo costs nothing: the endpoint reference is always
generated from exactly what the API is serving, so it cannot drift from the
service even though the two live in different repositories. An earlier setup kept
a second copy of the spec alongside the docs and it drifted within a day.

**Consequences of that choice:**

- A spec change appears here only after the API is deployed. Deploy the service
  first, then push docs.
- If Mintlify builds while `api.meowmail.in` is unreachable, the build fails
  rather than publishing a reference for endpoints that do not exist. That is the
  behaviour we want.

## What lives where

| Change | Where to make it |
|---|---|
| Guide prose, navigation, theme | **Here**, in the `.mdx` files and `docs.json` |
| Endpoint schemas, parameters, response shapes | The service repo, in `priv/openapi.json` — never here |
| Anything about how the API behaves | The service repo |

If you are editing an endpoint's request or response documentation and you are in
this repo, you are in the wrong place.

## Local preview

```bash
npm i -g mint
mint dev          # http://localhost:3000
```

## Custom domain

`docs.meowmail.in` is a CNAME pointing at the target shown in the Mintlify
dashboard, with the domain registered there. On Cloudflare the record must be
DNS-only (grey cloud), not proxied, or Mintlify cannot complete certificate
issuance.

## Contributing

Corrections and clarifications are welcome — open a PR. For anything that
requires the API itself to change, or to report abuse or request higher rate
limits, email dev.jaythorat@gmail.com.
