# AquaRev Certification Map — Deployment

Lives at **www.aquarevwater.us/certificationmap** (Webflow). Same split-deploy
pattern as the Data and Water Hardness pages: CSS + JS hosted on GitHub Pages,
a short embed pasted into Webflow. The block paints its own background and
every style is scoped to `#arcm`, so Webflow's global styles do not reach it.

The page is the interactive **Compliance Atlas** (world map, US-states map,
table). It links to the full **Global Compliance Map** document, which is hosted
as a standalone page on the same GitHub Pages repo.

## Masters (edit these, never the generated files)

| Master | Location |
|---|---|
| Atlas (interactive map) | `Marketing/Collateral/Certification/AquaRev_Compliance_Atlas_V1.html` |
| Compliance Map (document) | `Marketing/Collateral/Certification/AquaRev_Global_Compliance_Map_V1.html` |
| Research and internal notes | `Marketing/Collateral/Certification/Research/` (never published) |

## Generated files (this folder)

| File | Role |
|---|---|
| `certification-map.css` | Hosted on GitHub Pages. Atlas styles, every selector scoped under `#arcm`; palette tokens on `#arcm`, dark forced by `data-theme="dark"`. |
| `certification-map.js` | Hosted on GitHub Pages. Injects the atlas markup into `<div id="arcm">`, then holds the 70-jurisdiction and 51-state datasets and all logic. |
| `compliance-map.html` | Hosted on GitHub Pages. The full document as a standalone page; the atlas records link into its anchors. |
| `webflow-embed-certification-map.html` | Paste into the Webflow Embed element (under 1 KB). |
| `preview.html` | Local preview that mirrors the embed with local files. |
| `build.sh` | Regenerates all of the above from the masters. |

Map geometry comes from `datamaps.all.min.js` on cdnjs (world and US-state
topology); d3 v3.5.17 and topojson v1.6.9 also load from cdnjs. Nothing else is
fetched at runtime.

## GitHub Pages

- Repo: `jeffatley-web/aquarev_certification_map` (public), Pages source `main` branch, root.
- https://jeffatley-web.github.io/aquarev_certification_map/certification-map.css
- https://jeffatley-web.github.io/aquarev_certification_map/certification-map.js
- https://jeffatley-web.github.io/aquarev_certification_map/compliance-map.html

## Webflow page notes

- Drop an Embed element into a full-width section on `/certificationmap` and
  paste the contents of `webflow-embed-certification-map.html`. Publish.
- Give the section a dark background (`#0E171E` matches the block) or let the
  block paint itself; either works.
- If the site has a fixed nav, add this to the page's custom code head so the
  sticky toolbar and record panel sit below it rather than under it:
  `<style>#arcm{--arcm-sticky-top:72px}</style>` (use the nav's real height).
- Fonts (Archivo, Source Sans 3, IBM Plex Mono) load from Google Fonts via the
  first `<link>` in the embed. If those families are already in Webflow's font
  settings, that line can be removed.

## Update workflow

1. Edit the master(s) in `Marketing/Collateral/Certification/`.
2. Regenerate:

   ```bash
   cd "/Users/jatley/Documents/Jeff Atley/Aquarev Water /Marketing/Website/Certification Map"
   ./build.sh
   ```

3. Commit and push the generated files to the GitHub Pages repo:

   ```bash
   cd "/Users/jatley/Documents/Jeff Atley/Aquarev Water /Marketing/Website/Certification Map"
   git add certification-map.css certification-map.js compliance-map.html README.md build.sh webflow-embed-certification-map.html preview.html
   git commit -m "Update certification map"
   git push
   ```

4. The embed does not change unless the hosted URLs move. `build.sh` stamps a
   fresh `?v=` cache buster into the embed each run; re-paste it into Webflow
   when you want browsers to pick up a change immediately.

The published Claude artifact versions (light and dark themes) are the visual
reference: atlas https://claude.ai/artifact/MJug8Z6Be3f7m8a4jhPTup and
document https://claude.ai/artifact/5e3h82FST3RaScjGqJqVT9.

## Before publishing the Webflow page live

The internal open-items list is in
`Marketing/Collateral/Certification/Research/00_INTERNAL_Open_Items.md` and is
not part of any published file. Close or accept those items before the page
goes live.
