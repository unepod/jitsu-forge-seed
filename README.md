# jitsu-forge-seed

Public seed asset for [Jitsu Forge](https://github.com/unepod/jitsu-forge) — preset YouTube URLs for in-app techniques.

## URLs

- **Primary** — https://unepod.github.io/jitsu-forge-seed/v1/seed.json (GitHub Pages)
- **Mirror** — https://cdn.jsdelivr.net/gh/unepod/jitsu-forge-seed@main/v1/seed.json (jsDelivr CDN — China-friendly fallback per CN connectivity decision in `unepod/jitsu-forge` `.planning/phases/01-beta-block-bug-fixes-build-reset/01-CONTEXT.md` D-01)

## Schema (v1)

```json
{
  "version": 1,
  "urls": {
    "<techniqueId>": ["<youtube-url-1>", "<youtube-url-2>"]
  }
}
```

App contract:

- Client `clientMaxSeedVersion = 1`. Payloads with `version > 1` are skipped silently (graceful schema evolution per D-07).
- URLs in this payload **REPLACE** the bundled `assets/JSON/techniques.json` URLs **per `techniqueId`** (per D-06 + #117 stated goal). Per-`techniqueId` omission falls through to bundled URLs.
- User-added URLs (in-app APPEND) and hide-flags are **always preserved** — remote payload writes to a separate Hive box (`preset_video_urls`).
- Fetch cadence: stale-while-revalidate with a 24-hour floor (D-02). Cold start uses cache; background fetch fires only if `lastSeedFetchAt` is older than 24h.

## Updating

Edit `v1/seed.json`, commit to `main`. Live within seconds via GitHub Pages; jsDelivr CDN refreshes within ~5 minutes (`@main` ref points to head of branch).

Schema evolution: introduce `v2/seed.json` with `version: 2` when shape changes (e.g., locale-keyed URLs). Older clients silently fall back to bundled.

## Operational notes

- This is a **public asset**, not a credential. URLs above are safe to commit to client code (per `unepod/jitsu-forge` `CLAUDE.md` non-negotiable #1).
- No auth, no PII, no secrets in this repo or in payloads.

## Related

- App repo: https://github.com/unepod/jitsu-forge
- Tracking issue: https://github.com/unepod/jitsu-forge/issues/117
- Broken-URL incidents this seed payload corrects: https://github.com/unepod/jitsu-forge/issues/106 / https://github.com/unepod/jitsu-forge/issues/107 / https://github.com/unepod/jitsu-forge/issues/108
