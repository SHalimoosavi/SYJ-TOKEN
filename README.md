# SYJ Token — Official Website

Official website for the SYJ Token, the planned utility token for **SAYANJALI BLOCKCHAIN**, an independent Layer-1 network built by SAYANJALI NEXUS PRIVATE LIMITED. A single-file static site — no build step, no framework — deployed on GitHub Pages and maintained entirely from Android via Termux.

**Live site:** `https://shalimoosavi.github.io/SYJ-TOKEN/`
**Blockchain repo:** `https://github.com/SHalimoosavi/SAYANJALI-BLOCKCHAIN`
**GitHub:** `https://github.com/SHalimoosavi/SYJ-TOKEN`

---

## Overview

This site is the pre-launch hub for the SYJ Token: what it's for, how SAYANJALI BLOCKCHAIN works, the planned tokenomics, the roadmap, and where to follow real updates. **The token has not launched.** Every page is written to make that status impossible to miss — no live price ticker, no "Buy" button, no fabricated market data. Status labels ("Planned", "Pre-Launch", "Coming Soon") stay attached to anything not yet real.

**This is a hard rule for all future edits to this file, not just a starting choice.** If you're ever tempted to add a price ticker, a "Buy SYJ" button, exchange listings, or specific token launch dates before they're actually confirmed and legally cleared — don't. A live-looking price feed for an unlaunched token is a pattern strongly associated with scam sites, even when unintentional, and it's the fastest way to lose credibility with anyone doing real diligence.

---

## Folder Structure

```
/
├── index.html        ← the entire site (HTML + CSS + JS, single file)
├── robots.txt         ← crawler rules + sitemap pointer
└── sitemap.xml         ← tells search engines what to index
```

Everything — layout, styling, Three.js network animation, GSAP scroll reveals, and interactivity — lives inside `index.html`. No `/src`, no `node_modules`, no build pipeline, so it stays editable from a phone with `nano` and `git`.

---

## Termux Setup

```bash
pkg install git -y
git clone https://github.com/SHalimoosavi/SYJ-TOKEN.git
cd SYJ-TOKEN
```

Local preview (optional):
```bash
pkg install python -y
python -m http.server 8000
# open http://localhost:8000
```

---

## Editing Content

Sections, in order, by `<section id="...">`:

| Section ID | What it holds |
|---|---|
| `hero` | Headline, pre-launch status strip, CTA buttons |
| `tokenomics` | Planned allocation pie chart |
| `supply` | Planned vesting schedule chart |
| `features` | 8 planned utility cards (fees, governance, AI access, etc.) |
| `roadmap` | Phase timeline (done / active / upcoming) |
| `ecosystem` | Network diagram of ecosystem products |
| `education` | Planned SYJ Academy |
| `whitepaper` | Whitepaper status + link to blockchain source |
| `developers` | Open-source project grid (GitHub links) |
| `faq` | FAQ accordion (also feeds AEO/GEO schema) |
| `community` | Social links |
| `contact` | Contact emails + ecosystem links |

Find a section fast:
```bash
grep -n 'id="roadmap"' index.html
```

### Updating the roadmap
Edit the `phases` array inside the `<script>` block (search `var phases`). Each phase has a `status` of `'done'`, `'active'`, or `'upcoming'` — this controls the badge shown (✓ SHIPPED / ● ACTIVE / ○ UPCOMING). Move a phase from `upcoming` → `active` → `done` as things actually ship — don't mark something done that isn't.

### Adding a new FAQ item
Duplicate a `.faq-i2` block inside `#faq`, **and** add a matching entry to the `FAQPage` JSON-LD block in `<head>` — keep both in sync.

### Adding a new open-source project card
Copy a `.dev-card` block inside `#developers`, point `href` at the repo or live URL.

### Changing colors
CSS variables at the top of `<style>`:
```css
:root{
  --primary: #00ff88;
  --secondary: #00c8ff;
  --accent: #7b61ff;
  --bg: #050816;
}
```

---

## Deploying to GitHub Pages

```bash
cd SYJ-TOKEN
git add index.html robots.txt sitemap.xml
git commit -m "Update site content"
git push origin main
```
Rebuilds automatically in 1–2 minutes. First-time setup: repo → Settings → Pages → Source → `main` branch, root folder.

---

## SEO / AEO / GEO Checklist

- [x] `robots.txt` + `sitemap.xml` in repo root
- [x] Canonical URL set in `<head>`
- [x] `Organization`, `Person`, `WebSite`, `WebPage`, `SoftwareApplication`, `BreadcrumbList`, `FAQPage` JSON-LD
- [x] Question-first FAQ headings, answer-first paragraphs (optimized for AI Overviews / ChatGPT / Perplexity extraction)
- [ ] Submit `sitemap.xml` in Google Search Console (add this property separately from your other sites — different URL prefix)
- [ ] Request indexing on the homepage URL via URL Inspection
- [ ] Update JSON-LD dates/status as TGE progresses

---

## Performance & Accessibility Checklist

- [x] Fonts preconnected
- [x] `prefers-reduced-motion` respected
- [x] Particle canvas disabled on mobile (reduces load + fixes layout risk)
- [ ] Run Lighthouse before each major content push — Three.js/GSAP can regress scores if new sections are added carelessly

---

## Troubleshooting

**Mobile layout breaks (overlapping hero text/buttons):** `#hero` uses `display:flex` with no explicit direction, meaning **row** by default. Any element that becomes `position:static` inside it (like `.hero-stats` on mobile) becomes a flex row item, not a stacked block. If you add new static children to `#hero`, make sure `flex-direction:column` is set in the mobile media query, or the layout will scramble exactly like it did before.

**Footer links overflow off-screen:** `.ftr-links` needs `flex-wrap:wrap` — if you add more footer links later and this isn't set, the last ones will run off the right edge on narrow screens.

**Layout looks broken after an edit:** check for an unmatched `<div>`:
```bash
python3 -c "
c=open('index.html').read()
print('div open', c.count('<div'))
print('div close', c.count('</div>'))
"
```
Both numbers must match.

---

## Roadmap

- [x] SAYANJALI BLOCKCHAIN MVP shipped (v0.1.0-mvp, 37 tests)
- [x] Token website live, pre-launch status accurate
- [ ] Whitepaper v1.0 published — once live, update the `#whitepaper` section to link the actual PDF instead of the "in progress" state
- [ ] P2P networking, public testnet
- [ ] TGE — when scheduled, update hero, ticker, and FAQ answers together; don't let one go stale while the others update
- [ ] Verify/fix Telegram, X, and Discord links in `#community` — confirm each points to a real, active account before heavy promotion
