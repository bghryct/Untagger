# Untagger

Tag coverage for every Google Fonts family, built from the official tag database ([`google/fonts` → `tags/all/families.csv`](https://github.com/google/fonts/blob/main/tags/all/families.csv)). Use it to find what still needs tagging in the [GF Tagger](https://googlefonts.github.io/tagger/).

**Live site:** https://bghryct.github.io/Untagger/

## Views

- **All**: every family in the [Google Fonts catalog](https://fonts.google.com/metadata/fonts) with its tagging percentage, least tagged first.
- **Partly tagged**: families with some tags but below 100%.
- **Untagged**: families with no rows in `families.csv`.

Filter any view by name, category, gap type or percentage range. The search box also takes tag paths: `/Sans/Humanist` finds families with that tag, and `!/Quality` finds families without one. Filters are saved in the URL, so a filtered view can be shared as a link. **Compact** hides the font previews.

Each row has a **Tags** panel listing every tag with its score (VF marks variable-font tags scored at several axis positions), plus the Expressive tags it hasn't been scored on. Click a tag to list the families that have it. The current list can be copied or downloaded as TXT, CSV or JSON.

## How the percentage works

It's the average of three parts:

| Part | Full marks when |
| --- | --- |
| Quality | all `/Quality` tags are scored (99.9% of tagged families have all 4) |
| Expressive | at least as many `/Expressive` tags as 3 in 4 tagged families have (currently 7 of 20); taggers pick these selectively, so nobody is expected to have all 20 |
| Classification | it has a tag in the group its category calls for: Sans Serif → `/Sans`, Serif → `/Serif` or `/Slab`, Handwriting → `/Script`, Monospace → `/Monospace`, Display → any classification group |

**Flagged gaps** are the likely misses: a missing Quality tag, 3 or fewer Expressive tags, or no classification tag. Families with a `/Special use` tag (barcodes, redaction, symbols) get full marks for Expressive and Classification. The thresholds are computed from the live data each time the page loads.

## How it works

It's a single static `index.html` with no build step. On load it fetches:

| Data | Tried in order |
| --- | --- |
| Catalog | fonts.google.com (blocked by CORS in browsers) → the GF Tagger's daily copy → google.github.io copy → allorigins / corsproxy.io |
| Tags | raw.githubusercontent.com → GitHub API → allorigins / corsproxy.io |

Results are cached in `localStorage` for 24 hours. **Refresh** bypasses the cache and has a 30-second cooldown. If every source fails, the page shows the last cached copy, or you can load the two files by hand.
