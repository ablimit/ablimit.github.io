# Resume Website Refresh — Design

**Date:** 2026-07-02
**Owner:** Ablimit A. Keskin
**Branch:** update-kouper

## Goal

Refresh a stale (2018-era) Jekyll + Bootstrap 3 personal resume site so it accurately
reflects Ablimit's current identity — CTO & Co-founder at Kouper — and modernize the
visual presentation. Content accuracy is the primary constraint: this is a public
professional identity.

## Guiding constraints

- **User's own intro is canon.** Web results are enrichment only, used for the awards
  and Kouper funding facts.
- **Awards represented by their exact, verifiable public titles** (no inflation):
  - VLDB **Test of Time Award** (2024) — 2013 Hadoop-GIS paper, co-authored.
  - SIGSPATIAL **Best Poster** (2016) — "Scalable 3D Spatial Queries for Analytical
    Pathology Imaging with MapReduce."
  - Pathology Informatics **Best Presentation** (2012) — "MIGIS."
- Aggregator roles (Ionpath, Polarr) included but understated in the CV timeline.
- Contact: **LinkedIn-only** (plus GitHub / Google Scholar). No email published.
- Remove the stale "Download CV" PDF button.
- Modernize styling via a **custom CSS layer only** — do not edit Bootstrap files
  (reversible).

## Verified facts (sources)

- Kouper: emerged from stealth 2025, **$10M** funding (General Catalyst, 25Madison,
  CVS Health Ventures); vertical **transitions-of-care** solutions for health systems,
  payors, risk-bearing entities. Co-founded with Salman.
- Google Scholar (Ablimit Keskin / Aji): **~1,432 citations, h-index 14, i10-index 14**.
  Top works: Hadoop-GIS (VLDB 2013, ~866 cites), SATO (SIGSPATIAL 2014), medical-imaging
  spatial query systems.
- VLDB Test of Time 2024 authors: Zhang, Lee, Aji, Wang, Vo, Liu, Saltz.

## Scope of changes

### 1. Global config & chrome
- `_config.yml`: `description` → CTO & Co-founder framing; fix LinkedIn to `in/ablimit`.
- `_includes/head.html`: `<title>` → "… | CTO & Co-founder @ Kouper"; refresh keywords.
- `_includes/footer.html`: © 2026.
- `_includes/header.html`: nav name → "Ablimit A. Keskin"; **remove Notes & Projects
  nav items** (keep CV, Contact).

### 2. Homepage (`index.html`)
Hero + narrative built only from the user's intro:
- Lead: name + "CTO & Co-founder at Kouper."
- Narrative: through-line of healthcare + data infrastructure + applied AI; Emory PhD
  (databases + computational pathology); Komodo as formative data-foundation work; the
  belief that durable healthcare companies are built on deep data infrastructure; Kouper
  now (AI-native care navigation — voice/SMS on a deeper platform: shared patient
  context, EHR integration, agent orchestration, QA, safety).
- Selected recognition callout: the three awards (exact titles) + Scholar metrics.
- Links: LinkedIn, GitHub, Google Scholar.

### 3. CV (`cv/index.html`)
- Header: name + "CTO & Co-founder, Kouper." Remove physical address block.
- Honors section near top: the three awards, exact titles, one-line context each.
- Experience (reverse chronological):
  - **Kouper** — CTO & Co-founder (present). AI-native care navigation platform.
  - **Komodo Health** — early core technical member / engineering lead; longitudinal
    patient-level data foundation.
  - **Ionpath, Polarr** — data engineering / science (brief, understated).
  - **Hewlett Packard Labs** — Research Scientist; memory-driven compute, petabyte-scale.
  - **Emory University** — PhD research; Hadoop-GIS, computational pathology.
  - Internships: Yahoo! Search, Microsoft Research, Max-Planck Institute, Microsoft.
- Education: PhD & MS Emory; BE USTB.
- Publications: Scholar link + metrics; highlight Hadoop-GIS and SATO.
- Academic service kept.
- Remove Download CV button.

### 4. Projects → folded in, page removed
- Delete `/projects` and `/notes` pages (and nav items). Reuse project substance
  (Hadoop-GIS, SATO, MaReIA/medical-imaging systems) as achievement highlights in the
  CV publications/experience, not as a standalone list.

### 5. Styling (new custom CSS layer in `css/style.css` or a new partial)
Layered over Bootstrap, no framework replacement:
- Modern type scale and font pairing; generous line-height and spacing.
- Refined hero (avatar + lead), nav pills, footer.
- Subtle accent color; muted palette; responsive.
- Reversible: only additive overrides, Bootstrap files untouched.

## Out of scope
- Regenerating `files/cv-aji.pdf`.
- The stale 2015 `_posts` entry (leave or remove — no linked nav after Notes removal).
- Any content beyond the user's narrative + verified enrichment facts.

## Success criteria
- No reference to Komodo/HP Labs as *current*; Kouper is front and center.
- All three awards present with exact, verifiable titles.
- No email on the public page; LinkedIn link fixed.
- Site renders cleanly and looks modern on desktop and mobile.
- Bootstrap core files unmodified.
