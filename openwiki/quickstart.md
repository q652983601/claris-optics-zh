---
type: Project Overview
title: CLARIS Optics — Chinese demo site
description: Entry point for the CLARIS Optics documentation. Describes a static single-page marketing site (zh-CN) for near-zero-stress polycarbonate optical parts for polarized AR displays, built as one self-contained index.html plus a small assets folder.
tags: [claris-optics, marketing-site, static-html, ar, optics, chinese]
---

# CLARIS Optics — Chinese demo site

## What this repository is

This repository is the **Chinese-language (zh-CN) marketing demo** for CLARIS Optics, a manufacturer of near-zero-stress polycarbonate (PC) optical parts produced by injection compression molding in Danyang, Jiangsu, China. The parts target **polarization-sensitive AR display optics**.

The deliverable is deliberately minimal: a single hand-written `index.html` containing all markup, CSS, and JavaScript, plus an `assets/` folder of photographs and one video loop. There is **no build step, no package manager, no framework, and no test suite**. Opening the file is the whole dev loop.

Read [page architecture](page-architecture.md) for how the page is put together, and [interactions](interactions.md) for the scroll-driven polarization demo that is the centerpiece of the page.

## What the product is and why the page is built this way

The product claim is **residual stress in injection-molded optics destroys polarized display light** — it washes out brightness, shifts color, and blurs the image. CLARIS's process releases stress at the moment of molding, so polarized display light arrives as designed.

The entire page is built around making that claim *visually verifiable* rather than asserted in text. The core trick is a photographic comparison of the same optical family under crossed polarizers:

- A **stressed** PC lens shows vivid rainbow stress fringes (`assets/stress2_001.jpg`).
- A **CLARIS near-zero-stress** sample shows uniform extinction (`assets/pure2_001.jpg`).

The page lets the visitor *scroll* between the two images so the difference is experienced, not just described. That scroll interaction is the site's primary product demonstration and is documented in detail in [interactions](interactions.md).

## The narrative arc of the page

| Order | Section | Purpose |
|---|---|---|
| 1 | Hero | Brand + one-line positioning ("应力无形。直到它毁掉光。") over a looping video of lenses, with the manufacturing origin and location. |
| 2 | Rainbow test (`#test`) | The scroll-driven crossed-polarizer comparison described above. |
| 3 | Manifesto | A short poetic statement of the problem and the process response — the emotional hinge between demo and product. |
| 4 | Applications (`#apps`) | Three AR use cases where residual stress matters: myopia insert lenses, visor/face-window lenses, and waveguide substrates. |
| 5 | Evidence | A real factory photograph (`assets/real-comparison.jpg`) showing stressed vs. stress-free samples side by side under crossed polarizers. Captioned as a production-line shot. |
| 6 | Specs (`#specs`) | A material/process/optical specification table. |
| 7 | Contact (`#contact`) | Sample-request call to action, `mailto:sales@claris-optics.com`, 7-day global sample shipping. |

The nav bar (fixed, blurred backdrop) links to `#test`, `#apps`, `#specs`, and `#contact`. There is no link to the manifesto, evidence, or hero — the visitor is expected to scroll.

## Key product facts as stated on the page

These are the site's own claims, reproduced so future agents know what the page asserts. They are **marketing claims on a demo page**, not verified test data, and are not backed by any data files in this repository.

- Material: optical-grade polycarbonate (PC)
- Process: injection compression molding with a proprietary low-stress cycle
- Birefringence (phase delay): ≤ 20 nm target, per-batch reports available
- Transmittance: ≥ 89% across 400–780 nm
- Forms: curved lenses (in production) · flat substrates (in certification)
- Capacity: currently fully booked, with a window open for new partners
- Compliance: ANSI Z87.1 reachable · FDA / CE path in progress
- Waveguide substrate: ≤ 20 nm phase delay, for nanoimprint grating writing — certification in progress

Two status claims are deliberately forward-looking rather than factual: the waveguide substrate is marked "认证推进中" (certification in progress) and compliance is "路径推进中" (path in progress). Preserve that hedging when editing copy.

## Assets

All assets live in `/assets`. Only five of the eight files are referenced by `index.html`; the rest are unused backups.

| File | Dimensions | Role | Referenced |
|---|---|---|---|
| `hero-loop.mp4` | MP4, autoplay loop | Hero background video (muted, `playsinline`) | yes |
| `hero2_001.jpg` | 1280×720 | Hero still background, layered under the video | yes |
| `stress2_001.jpg` | 1280×720 | Stressed PC lens under crossed polarizers — start of the rainbow test | yes |
| `pure2_001.jpg` | 1280×720 | CLARIS stress-free sample — end of the rainbow test | yes |
| `real-comparison.jpg` | 1080×1440 | Factory evidence photo, stressed vs. stress-free | yes |
| `hero-lens_001.jpg` | 1280×720 | Unused lens photo | no |
| `stress-free_001.jpg` | 1280×720 | Unused stress-free photo | no |
| `stress-rainbow_001.jpg` | 1280×720 | Unused rainbow-stress photo | no |

Note the naming: the *stressed* baseline used on the page is `stress2_001.jpg` (the `_2` suffix), not `stress-rainbow_001.jpg`. If you swap the comparison imagery, match that convention or update the `src` attributes in `index.html` accordingly.

## Working on this page

There is no dev server defined in the repo. In practice:

1. Edit `index.html` directly. All CSS is in a single `<style>` block in `<head>`; all JS is a single inline `<script>` at the end of `<body>`.
2. Preview by opening `index.html` directly in a browser, or serve the folder statically (`python3 -m http.server`). Serving is recommended only because the page uses no `fetch`/module loading — a plain `file://` open works.
3. Copy lives in Simplified Chinese throughout the body. Keep `lang="zh-CN"` in sync if you add other locales.
4. There is no test harness. Manual verification checklist: nav anchors jump to the right sections; the hero video autoplays muted and loops; scrolling the rainbow-test section transitions the label and headline at the midpoint; the video disappears if it fails to load; the layout holds at ≤ 640 px width.

Do not hand-edit `AGENTS.md`, `CLAUDE.md`, or `.github/workflows/openwiki-update.yml` — those are OpenWiki scaffolding. See [OpenWiki automation](operations/openwiki-automation.md) for how the wiki refresh works.

## Where to go next

- [Page architecture](page-architecture.md) — design tokens, section-by-section structure, responsive rules, footer and legal note.
- [Interactions](interactions.md) — the scroll-driven polarization demo, the midpoint copy swap, and the hero video fallback.
- [OpenWiki automation](operations/openwiki-automation.md) — the scheduled workflow that regenerates these pages.

## Backlog

- **Unused assets** (`assets/hero-lens_001.jpg`, `assets/stress-free_001.jpg`, `assets/stress-rainbow_001.jpg`) — no source reference anywhere in `index.html`; purpose (backup vs. future use) is not determinable from the repository.
- **English / other locale versions** — the commit is titled "中文版 demo", implying sibling locale sites exist outside this repository; none are present here.
- **Hosting and deployment target** — no build config, deploy workflow, or hosting metadata exists in the repo.
- **Analytics, cookie consent, and form handling** — the only conversion path is a `mailto:` link; no tracking or backend form endpoint is present.
- **Asset provenance and licenses** — no attribution, licensing, or model-release notes exist for the factory photography in `assets/real-comparison.jpg`.
- **Verification of numeric claims** — the ≤ 20 nm phase delay and ≥ 89 % transmittance figures have no supporting measurement data in this repository.
