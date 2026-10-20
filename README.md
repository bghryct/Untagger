# Untagger

Lists every Google Fonts family that has no entry in the official tag database ([`google/fonts` → `tags/all/families.csv`](https://github.com/google/fonts/blob/main/tags/all/families.csv)), so it's quick to see what still needs tagging in the [GF Tagger](https://googlefonts.github.io/tagger/).

**Live site:** https://bghryct.github.io/Untagger/

## What it shows

- **Untagged**: families in the [Google Fonts catalog](https://fonts.google.com/metadata/fonts) that have no rows in `families.csv`.
- **Partly tagged**: families that have some tags but are missing a whole group: no `/Expressive` tags, no `/Quality` tags, or no classification tag (nothing under `/Sans`, `/Serif`, `/Slab`, `/Script`, `/Theme`, `/Monospace` or `/Special use`).

Each row has a live preview, a link to the specimen page, and a **Tagger** button that copies the family name and opens the tagger. The current view can be copied or downloaded as TXT, CSV or JSON.

## How it works

It's a single static `index.html` with no build step. On load it fetches:

| Data | Tried in order |
| --- | --- |
| Catalog | fonts.google.com (blocked by CORS in browsers) → the GF Tagger's daily copy → google.github.io copy → allorigins / corsproxy.io |
| Tags | raw.githubusercontent.com → GitHub API → allorigins / corsproxy.io |

Results are cached in `localStorage` for 24 hours. **Refresh** bypasses the cache and has a 30-second cooldown. If every source fails, the page shows the last cached copy, or you can load the two files by hand.
