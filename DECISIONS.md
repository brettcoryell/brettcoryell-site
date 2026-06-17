# DECISIONS.md — brettcoryell-site

Architectural and design decisions for agents (Coda, Cadence, Lumen) working on this repo.
Read this before making any non-trivial change. These are rules, not suggestions.

---

## Stack & Deployment

- **Static only.** Plain HTML/CSS/JS — no framework, no build step, no Tailwind. Do not introduce any of these unless Brett explicitly asks.
- **Auto-deploys from `master`** via Vercel. One commit to master = one production deploy.
- **DNS lives at Hover.** A records point to `76.76.21.21`. Do not touch DNS unless Brett is present.
- **Local preview:** `python3 -m http.server 4242` from the repo root.

### Vercel subdomain CLI

The correct command for attaching a custom subdomain is:
```
vercel alias set <subdomain.brettcoryell.com> <vercel-deploy-url>
```
Do **not** use `vercel domains add` — that attempts to take over nameservers for the root domain.

---

## Live Subdomains

All four are live and should not be repointed without Brett's explicit instruction:

| Subdomain | App |
|---|---|
| `resume.brettcoryell.com` | ai-resume |
| `msm.brettcoryell.com` | market-signal-monitor |
| `tokenburn.brettcoryell.com` | token-burn |
| `polaris.brettcoryell.com` | career-router |

DNS: four CNAMEs at Hover pointing to `cname.vercel-dns.com`.

---

## Theme Architecture

### Three-layer token chain

1. **Primitive layer** — raw palette values declared as `--primitive-*` in `:root` (index.html). Never referenced directly by components.
2. **Semantic site layer** — role-named tokens (`--color-bg-page`, `--color-text-primary`, etc.) defined in `themes/default.css`. This is what components consume.
3. Components use `--color-*` semantic tokens and never raw hex or `--primitive-*` values directly.

### Cross-repo token rule

`themes/default.css` is **identical across all five repos**: brettcoryell-site, ai-resume, career-router, token-burn, market-signal-monitor.

> **If you add a new token name to `themes/default.css`, you must add it to all five repos in the same commit or session.** Do not add tokens to one repo only.

The comment inside `themes/default.css` restates this rule. DECISIONS.md is the enforcement point.

### Design Guide

The canonical **Site Design Guide v1.1** lives in Google Drive at ID `1XvkBGW3RL7zGOcmezC_QZsa_GAS6-MUrFEFpLTCrod8`. It is the authority on color palette, type scale, spacing, and component patterns. Read it before making visual changes.

---

## Design Hard Rules

These apply to every change, no exceptions without Brett's explicit approval:

- **No text-shadow** on any text, anywhere. Brett dislikes drop shadows on text broadly.
- **No hero gradient scrim** over the background image.
- **No dark mode toggle.** The site is light-mode only.
- **Section headers are serif** (Georgia, 1.6rem, weight 400). Do not switch to sans-serif.
- **CTA button is solid fill** — sky-deep (`#5b7d99`) background, white text, `brightness(0.88)` on hover. Not outlined.
- **No all-caps on label text.**
- **Project cards open in-place** — no `target="_blank"`.

---

## Hero Animation

The v16 animation sequence is **locked**. Do not change the order or timing:

1. Drift in
2. Brightness lift
3. Float
4. Nudge
5. Sonar

`prefers-reduced-motion` must be respected — the animation must not run when the user has reduced motion enabled.

The **photo credit** fades out when tagline line 4 floats up, and fades back in on scroll. This behavior is intentional; do not remove it.

---

## Site Structure

### Navigation
- Fixed nav, transparent over hero → white/blur on scroll.
- Top nav links: Writing, Projects, About.
- Footer nav: Projects, About, LinkedIn, Email (pipe-separated).
- Dynamic copyright year via JS.

### Hero
- Full-viewport background image (`xingchen-yan-lYBDJP9oSEk-unsplash.jpg`, Unsplash, attribution in photo credit element).
- Four-line serif italic tagline.
- See Hero Animation section above for animation rules.

### Ariel band
- Background: sky-pale (`#dce8f0`). Left border: sky-deep (`#5b7d99`). Text: ink-colored italic.
- Button links to `https://resume.brettcoryell.com`.

### Writing section
**Intentionally removed from the live page** pending real content.
- Markup is preserved verbatim in `_writing-section.html` at the repo root.
- To restore: paste the block back between the Ariel band and the Projects section; add "Writing" back to both the top nav and footer nav. All CSS classes (`.writing-grid`, `.writing-card`, etc.) are already in `index.html` — no CSS work required.

### Projects section
- Label: **"Things I've Built"** — Brett's voice. Do not normalize to "Projects" or similar.
- Current cards (in order): Market Signal Monitor, Token Burn, Talk to Ariel, Polaris.
- Cards link to their respective subdomains (see Live Subdomains above).

### About section
- Short paragraph, 68ch max-width.

---

## Deferred (not forgotten)

- **Site background color:** slightly creamier than pure white — Brett's idea, no timeline set.
