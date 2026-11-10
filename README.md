# Literary Universe for Grok

Grok plugin for the hosted MCP endpoint at `https://literaryuniverse.com/mcp`.
Sign-in uses Literary Universe's OAuth authorization server. Grok registers a
public client, opens the browser, and stores the refresh token locally. No API
key and no client secret belong in this package.

## What it can do

Fifteen named tools: story and chapter reads, account identity, universes,
encyclopedia reads, series, platform help, and encyclopedia creation, editing
and translation. The `assistant` category adds the hosted `load-writing-skill`
and `load-universe-skill` tools, which return the website AI Assistant's review
instructions with fresh story context.

`oauth_scopes` asks for `story:read`, `universe:read`, `universe:write` and
`universe:translate`. Without that list, Grok's client would request every
scope advertised on the protected resource, including publish. The URL's tool
selection controls discovery; the server still enforces the token's scopes on
every call. Named tools keep later SDK categories from widening the listing.
Manuscript writes, chapter publication, entry deletion and encyclopedia
publication stay out.

The `literary-assistant` skill loads the assistant's instructions from the
server for each task, so this repository holds no copy of them. Encyclopedia
creates, edits and translations wait for the author to approve the values shown
in chat.

## Install

```sh
grok plugin install LiteraryUniverse/grok-literary-universe --trust
grok plugin enable literary-universe
```

From a checkout: `grok plugin validate .` and `grok plugin install . --trust`.

Start a new Grok session. In `/mcps`, authenticate `literary-universe`. Consent
should name story read, universe read, universe write and universe translate.
An earlier read-only grant does not gain the new scopes until the author
approves them again. Revoke the connected app from Settings → API keys to
disconnect it.

## Requirements on the server

The review skill needs the hosted assistant tools
([Literary-Universe#1282](https://github.com/StorytellerCZ/Literary-Universe/pull/1282))
deployed on literaryuniverse.com.

## Verify the package

```sh
npm ci
npm test
```

The test checks the tool list against the SDK's real MCP registration and the
write and overwrite annotations. It does not sign in or call production.

Read-only production checks on 2026-11-09 confirmed protected-resource
discovery, the authorization server, dynamic registration and S256 PKCE. A
local Grok doctor against the same URL stopped at `Auth required` with that
discovery header. No production client, account or grant was created.
