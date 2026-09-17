# AquaRev Certification pages — Deployment

Two pages on **aquarevwater.us** (Webflow), same split-deploy pattern as the Data
and Water Hardness pages: CSS + JS hosted on GitHub Pages, a short embed pasted
into each Webflow page. Each block paints its own background and every style is
scoped to its container, so Webflow's global styles do not reach it.

| Page | Slug | Container | Files |
|---|---|---|---|
| Compliance Atlas (interactive map) | `/certificationmap` | `#arcm` | `certification-map.css`, `certification-map.js`, `webflow-embed-certification-map.html` |
| Global Compliance Map (document) | `/compliancemap` | `#arcd` | `compliance-map.css`, `compliance-map.js`, `webflow-embed-compliance-map.html` |

The atlas links to the document page (and deep-links into its cards, e.g.
`/compliancemap#fr`); the document links back to the atlas from its masthead.
If either slug changes, edit `SITE_MAP` / `SITE_DOC` at the top of `build.sh`,
rebuild and push.

## Masters (edit these, never the generated files)

| Master | Location |
|---|---|
| Atlas (interactive map) | `Marketing/Collateral/Certification/AquaRev_Compliance_Atlas_V1.html` |
| Compliance Map (document) | `Marketing/Collateral/Certification/AquaRev_Global_Compliance_Map_V1.html` |
| Research and internal notes | `Marketing/Collateral/Certification/Research/` (never published) |

## Generated files (this folder)

`build.sh` regenerates everything below from the masters. Each JS file injects
its page markup into the empty container, then runs the page logic. The
datasets (70 jurisdictions, 51 US states) live in `certification-map.js`.

Map geometry comes from `datamaps.all.min.js` on cdnjs (world and US-state
topology); d3 v3.5.17 and topojson v1.6.9 also load from cdnjs. The document
page loads nothing but its own CSS/JS and Google Fonts.

`preview.html` and `preview-doc.html` mirror the two embeds with local files.

## GitHub Pages

- Repo: `jeffatley-web/aquarev_certification_map` (public), Pages source `main` branch, root.
- https://jeffatley-web.github.io/aquarev_certification_map/certification-map.css
- https://jeffatley-web.github.io/aquarev_certification_map/certification-map.js
- https://jeffatley-web.github.io/aquarev_certification_map/compliance-map.css
- https://jeffatley-web.github.io/aquarev_certification_map/compliance-map.js

## Webflow page notes

- On each page, drop an Embed element into a full-width section and paste the
  matching `webflow-embed-*.html`. Publish.
- Give the section a dark background (`#0E171E` matches both blocks) or let the
  block paint itself; either works.
- If the site has a fixed nav, add this to each page's custom code head so the
  sticky bars and the record drawer sit below it rather than under it:
  `<style>#arcm{--arcm-sticky-top:72px}</style>` on the atlas page and
  `<style>#arcd{--arcd-sticky-top:72px}</style>` on the document page (use the
  nav's real height).
- Fonts (Archivo, Source Sans 3, IBM Plex Mono) load from Google Fonts via the
  first `<link>` in each embed. If those families are already in Webflow's font
  settings, that line can be removed.

## Update workflow

1. Edit the master(s) in `Marketing/Collateral/Certification/`.
2. Regenerate:

   ```bash
   cd "/Users/jatley/Documents/Jeff Atley/Aquarev Water /Marketing/Website/Certification Map"
   ./build.sh
   ```

3. Commit and push the generated files:

   ```bash
   cd "/Users/jatley/Documents/Jeff Atley/Aquarev Water /Marketing/Website/Certification Map"
   git add -A
   git commit -m "Update certification pages"
   git push
   ```

4. `build.sh` stamps a fresh `?v=` cache buster into both embeds each run.
   Re-paste an embed into Webflow when you want browsers to pick up a change
   immediately; otherwise the GitHub Pages cache refreshes within about ten
   minutes on its own.

The published Claude artifact versions (light and dark themes) are the visual
reference: atlas https://claude.ai/artifact/MJug8Z6Be3f7m8a4jhPTup and
document https://claude.ai/artifact/5e3h82FST3RaScjGqJqVT9.

## Before publishing the Webflow pages live

The internal open-items list is in
`Marketing/Collateral/Certification/Research/00_INTERNAL_Open_Items.md` and is
not part of any published file. Close or accept those items before the pages
go live.
