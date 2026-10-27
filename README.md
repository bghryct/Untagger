# Untagger

Tag coverage for every Google Fonts family, built from the official tag database ([`google/fonts` → `tags/all/families.csv`](https://github.com/google/fonts/blob/main/tags/all/families.csv)). Use it to find what still needs tagging in the [GF Tagger](https://googlefonts.github.io/tagger/).

**Live site:** https://bghryct.github.io/Untagger/

## Views

- **All**: every family in the [Google Fonts catalog](https://fonts.google.com/metadata/fonts) with its tagging percentage, least tagged first.
- **Partly tagged**: families with some tags but below 100% on the Expected measure.
- **Untagged**: families with no rows in `families.csv`.

Filter any view by name, category, gap type, Tagger % and Expected % (both percentage filters work whichever measure is shown). The search box also takes tag paths: `/Sans/Humanist` finds families with that tag, and `!/Quality` finds families without one. Filters are saved in the URL, so a filtered view can be shared as a link. Lists start in **Compact** layout; switch to **Previews** to see each family rendered in its own font.

Each row has a **Tags** panel listing every tag with its score (VF marks variable-font tags scored at several axis positions), plus the Expressive tags it hasn't been scored on. Click a tag to list the families that have it. The current list can be copied or downloaded as TXT, CSV or JSON.

## The two percentages

The **Measure** switch picks which one is shown and used for sorting.

**Tagger** (default) is the [GF Tagger](https://googlefonts.github.io/tagger/)'s own completeness: tags scored out of every tag the tagger tracks (75 today, from its `tags_metadata.csv` and `tag_definitions.json`, not counting `/quant` measurements). A variable-font tag counts once however many axis positions it has. The catalog-wide figure matches the tagger's Todo panel ("Tagging is …% complete"). Most tags only suit some fonts, so nobody gets near 100%: the median family is around 19%.

**Expected** averages three parts, so 100% means tagged to the usual standard:

| Part | Full marks when |
| --- | --- |
| Quality | all `/Quality` tags are scored (99.9% of tagged families have all 4) |
| Expressive | at least as many `/Expressive` tags as 3 in 4 tagged families have (currently 7 of 20); taggers pick these selectively, so nobody is expected to have all 20 |
| Classification | it has a tag in the group its category calls for: Sans Serif → `/Sans`, Serif → `/Serif` or `/Slab`, Handwriting → `/Script`, Monospace → `/Monospace`, Display → any classification group |

**Flagged gaps** are the likely misses: a missing Quality tag, 3 or fewer Expressive tags, or no classification tag. Families with a `/Special use` tag (barcodes, redaction, symbols) get full Expected marks for Expressive and Classification. Thresholds are computed from the live data each time the page loads.

## Tagging proposals

**Propose** on any row (or **Propose tags** for the list shown, up to 25 families) opens a printable proposal for each family:

- a specimen of the family, and its 10 most similar tagged families, each set in its own font, with why they're similar (same superfamily, related name, same designer, category, style, script, axes);
- the top three classification candidates side by side, each with fonts that carry that tag, to pick by eye;
- suggested Quality, Expressive and other tags with scores, the range across similar families, support and confidence;
- a 0–100 scale diagram per suggested tag, placing the family among similar families and well-known fonts at the low, middle and high end, all rendered in their real fonts;
- existing scores that disagree strongly with similar families, and ready-to-paste `families.csv` rows.

Suggestions are a similarity-weighted vote of the 10 nearest tagged families by catalog metadata (closer families count far more, so a sibling like Asap dominates for Asap Sharp). Each proposal says how far to trust it: the method is re-tested every time by hiding the tags of 150 already-tagged families and predicting them. At the time of writing, suggested Expressive scores are off by about 15 points on average (21 using each tag's average), Quality by about 7, and the right classification is among the top three about 80% of the time.

## Severity report

**Severity report** opens a printable report of where tagging is lacking:

| Level | Rule |
| --- | --- |
| Critical | No rows in `families.csv` |
| High | Missing a `/Quality` tag, or a Sans Serif, Serif, Handwriting or Monospace family with no tag in its classification group |
| Medium | 3 or fewer `/Expressive` tags, or a Display family with no classification tag |
| Low | No flagged gaps, but in the bottom 10% of tagged families by tagger % |

It has key findings, recommended next steps (with links to proposals and filtered lists), a coverage histogram, one section per level listing every family in its own font (most-used first), and gaps by tag, by category and by year added. The list can also be sorted by **Most severe gaps**.

Both documents can be printed or saved as PDF, or downloaded as a standalone HTML file that loads the fonts it uses.

## How it works

It's a single static `index.html` with no build step. On load it fetches:

| Data | Tried in order |
| --- | --- |
| Catalog | fonts.google.com (blocked by CORS in browsers) → the GF Tagger's daily copy → google.github.io copy → allorigins / corsproxy.io |
| Tags | raw.githubusercontent.com → GitHub API → allorigins / corsproxy.io |
| Tagger's tag list | googlefonts.github.io/tagger → googlefonts/tagger on GitHub (if both fail, the tags found in families.csv) |

Results are cached in `localStorage` for 24 hours. **Refresh** bypasses the cache (a full refresh takes well under a second) and has a 5-second cooldown against double clicks. If every source fails, the page shows the last cached copy, or you can load the two files by hand.
