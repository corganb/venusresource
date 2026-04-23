# VenusResource - Claude Code Guide

## Site
- URL: https://venusresource.net
- Repo: https://github.com/corganb/venusresource
- Host: Cloudflare Pages (auto-deploys from `main`)
- Supabase project: `pxocavaczfjcpvqeiznt` (shared across all sibling sites)
- Engine: Three.js (WebGL, single-page)
- Cloudflare Pages project name: `venusresource-site`

## Repo layout
- `public/index.html` - main app (single-HTML)
- `public/resource-systems-core.js` - shared RS namespace library (identical file across sibling sites)
- `public/faq.html`, `public/privacy.html`, `public/terms.html`
- `public/_headers`, `public/robots.txt`, `public/sitemap.xml`

## Hard rules
1. **No em dashes.** Anywhere. Use hyphens, colons, or parentheses. Applies to code, UI text, comments, commit messages.
2. **Cache-bust core.js after edits.** Bump `?v=N` on the `<script src>` tag. Current: `v=17`.
3. **Verify syntax before pushing.** On the last `<script>` block, check `{` vs `}` balance and even backtick count.
4. **Commit early, commit often.** One logical change per commit.
5. **Python or Edit tool for multi-line edits.** Bash heredocs and repeated sed drop characters on this machine.
6. **Use centralized UI helpers, do not fork them per-site.** Top nav, footer links, and analysis panel all come from `RS.*` in core.js. If behavior needs to differ, add an option to the helper instead of duplicating code here.
7. **Real data only.** Spacecraft positions, body rotation, and orbital parameters must derive from real sources (ephemerides, NASA mission pages). No placeholders.
8. **Commit attribution: Corgan Studio, Inc. only.** Never add Co-Authored-By Claude.

## Shared RS.* API (core.js v=17)
- `RS.renderTopBar(opts)` - centralized top nav (used across all 9 Three.js siblings + galactic)
- `RS.renderFootLinks()` - consistent footer nav
- `RS.showAnalysisPanel(data)` - click-to-inspect detail panel
- `RS.openPanorama(imageUrl, title)` - 360 sphere viewer
- `RS.checkSession(onAuth)` - Supabase auth init
- `RS.fetchTier()` / `RS.isProUser()` / `RS.hasTier(minTier)`
- `RS.openUpgradeModal(reason, tier)`
- `RS.getPlanetPositionXYZ(name, date)` - shared ephemeris helper
- `RS.TIERS = { free:0, researcher:1, pro:1, team:2, enterprise:3 }`

## Venus quirks
- Spacecraft present in scene: Akatsuki, Magellan
- NASA skybox cubemap background
- Time-scale UI: 0x / 60x / 300x / 1000x, keyboard `Space` pause, `R` reset view, `1`-`4` time scales
- Sim time displayed in topbar

## Tier system
- `profiles.tier` in Supabase: `free` / `researcher` / `team` / `enterprise`
- Free: 3 active layers max. Researcher+: unlimited.

## Deploy
Cloudflare Pages auto-deploys on push to `main`.

## Commit style
- Format: `type: short description` (e.g. `feat:`, `fix:`, `nav:`, `chore:`)
- No em dashes, no Co-Authored-By Claude
- Attribution: Corgan Studio, Inc.
