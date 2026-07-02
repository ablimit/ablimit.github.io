# Resume Website Refresh Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Refresh the stale 2018-era Jekyll resume site to accurately present Ablimit as CTO & Co-founder of Kouper, with a modernized visual layer.

**Architecture:** Jekyll static site with Liquid templates and Bootstrap 3. All content edits are in `index.html`, `cv/index.html`, `contact/index.html`, and the `_includes`/`_config.yml` chrome. Styling is a purely additive layer appended to `css/style.css` — Bootstrap core files are never edited, keeping the change reversible.

**Tech Stack:** Jekyll, Liquid, Bootstrap 3, HTML5, CSS3.

## Global Constraints

- User's own intro narrative is canon; web results are enrichment only.
- Awards use exact verifiable titles: **VLDB Test of Time Award (2024)** (Hadoop-GIS 2013, co-authored); **SIGSPATIAL Best Poster (2016)**; **Pathology Informatics Best Presentation (2012)**.
- Scholar metrics: **~1,432 citations, h-index 14, i10-index 14** (approximate — use "~1,400+ citations, h-index 14").
- Experience year ranges (public sources): Kouper 2022–present; Ionpath 2021–2022; Polarr 2020–2021; Komodo 2017–2020; HP Labs 2014–2017; Emory PhD 2008–2014.
- Contact: **LinkedIn-only** — no email published anywhere. LinkedIn URL is `https://www.linkedin.com/in/ablimit/`.
- **Never edit** `css/bootstrap*.css`, `css/2.3.2/*`, or `js/*`. Style changes go only in `css/style.css`.
- Font `@import` URLs must be `https://` (existing ones use insecure `http://` — fix them).
- Remove the "Download CV" PDF button and the Notes/Projects nav items and pages.

## Verification note

Jekyll cannot build locally in this environment (system Ruby 2.6, no gem write access). Verification is therefore: (a) **static Liquid/HTML review** — confirm no undefined Liquid variables, balanced tags, valid links; (b) **CSS visual check** via a throwaway static HTML mock rendered in a browser (Playwright), never committed. GitHub Pages performs the authoritative build on push.

## File Structure

- `_config.yml` — MODIFY: site description, LinkedIn handle.
- `_includes/head.html` — MODIFY: `<title>`, keywords meta.
- `_includes/header.html` — MODIFY: nav name, remove Notes/Projects pills.
- `_includes/footer.html` — MODIFY: copyright year.
- `index.html` — REWRITE: hero + narrative + recognition callout.
- `cv/index.html` — REWRITE: experience, honors, education, publications, service.
- `contact/index.html` — MODIFY: LinkedIn-only.
- `projects/index.html`, `notes/index.html` — DELETE.
- `css/style.css` — MODIFY: fix font imports to https; APPEND modern styling layer.

---

### Task 1: Refresh site chrome (config, head, header, footer)

**Files:**
- Modify: `_config.yml`
- Modify: `_includes/head.html`
- Modify: `_includes/header.html`
- Modify: `_includes/footer.html`

**Interfaces:**
- Consumes: nothing.
- Produces: `page.cv` and `page.contact` are the only nav-state variables still referenced by `header.html` after Notes/Projects pills are removed. Every content page must set `cv` and `contact` front-matter keys (Task 2–4 rely on this).

- [ ] **Step 1: Update `_config.yml`**

Change the `description` line and the `linkedin` footer-link:

```yaml
# Short bio or description (displayed in the header)
description: CTO & Co-founder at Kouper. Building AI-native care navigation on deep healthcare data infrastructure.
```

```yaml
  linkedin: in/ablimit
```

- [ ] **Step 2: Update `_includes/head.html`**

Replace the `<title>` line and the keywords `<meta>`:

```html
	<title>{{ page.title }} | Ablimit A. Keskin — CTO &amp; Co-founder @ Kouper</title>
```

```html
	<meta name="keywords" content="Ablimit Keskin, Ablimit Aji, Kouper, care navigation, transitions of care, applied AI, healthcare data infrastructure, Komodo Health, VLDB Test of Time, Hadoop-GIS, Emory University, computational pathology">
```

- [ ] **Step 3: Update `_includes/header.html`**

Remove the Notes and Projects `<li>` pills and update the masthead name. Result:

```html
<div class="header">
	<ul class="nav nav-pills pull-right">
		<li class="{{ page.cv }}"><a href="/cv" title="CV">CV</a></li>
		<li class="{{ page.contact }}"><a href="/contact" title="Contact">Contact</a></li>
	</ul>
	<h3 class="text-muted"><a href="/">Ablimit A. Keskin</a></h3>
</div> <!-- /header -->
```

- [ ] **Step 4: Update `_includes/footer.html`**

Change the copyright line:

```html
	<p><small>&copy; Ablimit A. Keskin 2026</small></p>
```

- [ ] **Step 5: Verify Liquid validity**

Run: `grep -n "page\.\(notes\|projects\)" _includes/header.html`
Expected: no output (those variables are no longer referenced in nav).

Run: `grep -rn "{% *include" _layouts/default.html`
Expected: still references head, header, footer, scripts — all present.

- [ ] **Step 6: Commit**

```bash
git add _config.yml _includes/head.html _includes/header.html _includes/footer.html
git commit -m "Refresh site chrome: Kouper title, LinkedIn fix, nav cleanup, 2026 footer"
```

---

### Task 2: Rewrite homepage (`index.html`)

**Files:**
- Modify (rewrite body): `index.html`

**Interfaces:**
- Consumes: `page.cv` / `page.contact` nav-state (Task 1). Front matter must set `cv: passive` and `contact: passive`.
- Produces: nothing downstream.

- [ ] **Step 1: Rewrite `index.html`**

Replace the entire file. Front matter drops the removed `projects`/`notes` keys:

```html
---
layout: default
title: Ablimit A. Keskin
cv: passive
contact: passive
description: CTO & Co-founder at Kouper — AI-native care navigation built on deep healthcare data infrastructure.
---
<div class="row marketing intro">
	<div class="col-sm-4">
		<img class="img-thumbnail avatar" alt="Ablimit A. Keskin" src="img/avatar.jpg">
	</div>
	<div itemscope itemtype="http://schema.org/Person" class="col-sm-8">
		<p class="lead">Hi, I'm <span itemprop="name">Ablimit</span> — <span itemprop="jobTitle">CTO &amp; Co-founder</span> at <span itemprop="worksFor"><a href="https://www.kouperhealth.com">Kouper</a></span>.</p>

		<p>My work has consistently lived at the intersection of <strong>healthcare, data infrastructure, and applied AI</strong>. I did my Ph.D. in Computer Science at <a href="https://www.emory.edu">Emory</a>, working on large-scale database systems and computational pathology for cancer diagnosis and prediction — research later recognized with the <strong>VLDB Test of Time Award</strong>.</p>

		<p>I've spent the years since translating that foundation into real-world healthcare infrastructure. As an early core technical member at <a href="https://www.komodohealth.com">Komodo Health</a>, I led engineering teams building the longitudinal, patient-level data foundation that powers its product lines today. That work shaped a belief I still hold: in healthcare, the durable companies are built on deep data infrastructure long before they surface as applications.</p>

		<p>At Kouper, my co-founder Salman and I are applying that same foundation-first mindset to the <strong>AI-native care navigation layer</strong> for healthcare providers — starting with voice and SMS workflows, but grounded in a deeper platform: shared patient context, EHR integration, agent orchestration, QA, and safety infrastructure.</p>

		<p class="links">
			<a href="https://www.linkedin.com/in/ablimit/">LinkedIn</a> &middot;
			<a href="https://github.com/ablimit">GitHub</a> &middot;
			<a href="https://scholar.google.com/citations?user=P1KTbgkAAAAJ&hl=en">Google Scholar</a> &middot;
			<a href="/cv">CV</a>
		</p>
	</div>
</div>
```

- [ ] **Step 2: Verify no stale references and valid links**

Run: `grep -niE "komodo.*present|hewlett|research scientist|goodreads" index.html`
Expected: no output (no current-Komodo / HP framing remains).

Run: `grep -c "https://" index.html`
Expected: ≥ 4 (LinkedIn, GitHub, Scholar, Kouper/Emory/Komodo all https).

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "Rewrite homepage: lead with Kouper CTO role and healthcare/data/AI narrative"
```

---

### Task 3: Rewrite CV (`cv/index.html`)

**Files:**
- Modify (rewrite): `cv/index.html`

**Interfaces:**
- Consumes: `page.cv` / `page.contact` nav-state (Task 1). Front matter sets `cv: active`, `contact: passive`.
- Produces: nothing downstream.

- [ ] **Step 1: Rewrite `cv/index.html`**

Replace the entire file. No Download-CV button, no address block, honors near top:

```html
---
layout: default
title: CV
cv: active
contact: passive
description: CV for Ablimit A. Keskin — CTO & Co-founder at Kouper.
---
<div class="cv" itemscope itemtype="http://schema.org/Person">
	<h1><span itemprop="name">Ablimit A. Keskin</span></h1>
	<p class="cv-subtitle"><span itemprop="jobTitle">CTO &amp; Co-founder</span>, <span itemprop="worksFor">Kouper</span></p>

	<p>I build large-scale data-intensive systems and the teams behind them, with a decade-plus focused on healthcare data infrastructure and applied AI. My research background is in parallel database systems, spatial data management, and computational pathology.</p>

	<h2>Selected Honors</h2>
	<ul>
		<li><strong>VLDB Test of Time Award</strong> (2024) — for the 2013 paper <em>Hadoop-GIS: A High Performance Spatial Data Warehousing System over MapReduce</em> (co-authored).</li>
		<li><strong>Best Poster</strong>, ACM SIGSPATIAL (2016) — <em>Scalable 3D Spatial Queries for Analytical Pathology Imaging with MapReduce</em>.</li>
		<li><strong>Best Presentation</strong>, Pathology Informatics (2012) — <em>MIGIS: a high-performance query system for pathology imaging analytics</em>.</li>
	</ul>

	<h2>Professional Experience</h2>

	<p><strong>CTO &amp; Co-founder, Kouper, 2022 &ndash; present</strong></p>
	<ul>
		<li>Co-founded Kouper to build the AI-native care navigation layer for healthcare providers — voice and SMS workflows grounded in a deeper platform of shared patient context, EHR integration, agent orchestration, QA, and safety infrastructure.</li>
		<li>Kouper emerged from stealth in 2025 with $10M in funding from General Catalyst, 25Madison, and CVS Health Ventures.</li>
	</ul>

	<p><strong>Data Engineering &amp; Science, IONpath, 2021 &ndash; 2022</strong></p>
	<ul>
		<li>Built data engineering and analytics infrastructure for multiplexed tissue imaging.</li>
	</ul>

	<p><strong>Data Engineering, Polarr, 2020 &ndash; 2021</strong></p>
	<ul>
		<li>Developed data infrastructure supporting large-scale machine-learning workflows.</li>
	</ul>

	<p><strong>Engineering Manager, Komodo Health, 2017 &ndash; 2020</strong></p>
	<ul>
		<li>Early core technical member; led engineering teams building the longitudinal, patient-level data foundation that connects fragmented healthcare data into usable care journeys and powers Komodo's product lines.</li>
	</ul>

	<p><strong>Research Scientist, Hewlett Packard Labs, Palo Alto, 2014 &ndash; 2017</strong></p>
	<ul>
		<li>Built petabyte-scale systems on memory-driven compute architecture, including <a href="https://community.hpe.com/t5/Behind-the-scenes-Labs/Hewlett-Packard-Labs-accelerates-finance-with-blazingly-fast/ba-p/6920418">fast derivatives pricing</a>.</li>
	</ul>

	<p><strong>Research Assistant, Emory University, Atlanta, 2008 &ndash; 2014</strong></p>
	<ul>
		<li>Designed and implemented <em>Hadoop-GIS</em>, a MapReduce-based spatial query system, later recognized with the VLDB Test of Time Award; open-sourced as <em>HiveSP</em> and <em>libhadoopgis</em>.</li>
		<li>Built high-performance spatial query systems for large-scale analytical pathology imaging, applied to cancer diagnosis and prediction.</li>
	</ul>

	<p><strong>Earlier</strong>: Research/Engineering internships at Yahoo! Search, Microsoft Research (Redmond), Max-Planck Institute for Informatics, and Microsoft (Beijing).</p>

	<h2>Education</h2>
	<p><strong>Ph.D., Computer Science, Emory University</strong>, 2014<br>
	<strong>M.S., Computer Science, Emory University</strong>, 2010<br>
	<strong>B.E., Computer Science, University of Science and Technology Beijing</strong>, 2006</p>

	<h2>Selected Publications</h2>
	<p>~1,400+ citations, h-index 14 — full list on <a href="https://scholar.google.com/citations?user=P1KTbgkAAAAJ&hl=en" title="Google Scholar">Google Scholar</a>.</p>
	<ul>
		<li><em>Hadoop-GIS: A High Performance Spatial Data Warehousing System over MapReduce</em>, VLDB 2013 — <strong>Test of Time Award, 2024</strong>.</li>
		<li><em>SATO: a spatial data partitioning framework for scalable query processing</em>, ACM SIGSPATIAL 2014.</li>
		<li><em>High performance spatial queries for spatial big data: from medical imaging to GIS</em>, SIGSPATIAL Special 2015.</li>
	</ul>

	<h2>Academic Service</h2>
	<ul class="list-unstyled">
		<li><em>PC Member:</em> SIGSPATIAL 2014/2015/2016, SIGSPATIAL Ph.D. Symposium 2014, HealthGIS 2015</li>
		<li><em>Invited Journal Reviewer:</em> IEEE TOBD, TODS, JCST, CMPB, IJGIS, CCPE</li>
		<li><em>Reviewer:</em> SIGSPATIAL 2013, BigSpatial 2013, VLDB 2013/2014/2015, IEEE Big Data 2014</li>
	</ul>
</div>
```

- [ ] **Step 2: Verify no stale artifacts**

Run: `grep -niE "download cv|brannon|434|Komodo Health<br>|present</strong>.*Komodo" cv/index.html`
Expected: no output (button and address gone; Komodo no longer "present").

Run: `grep -c "Test of Time" cv/index.html`
Expected: ≥ 2 (honors + publications).

- [ ] **Step 3: Commit**

```bash
git add cv/index.html
git commit -m "Rewrite CV: Kouper-first timeline, three honors, updated publications"
```

---

### Task 4: Update contact page and remove dead pages

**Files:**
- Modify: `contact/index.html`
- Delete: `projects/index.html`
- Delete: `notes/index.html`

**Interfaces:**
- Consumes: `page.cv` / `page.contact` nav-state (Task 1).
- Produces: nothing downstream.

- [ ] **Step 1: Rewrite `contact/index.html`** (LinkedIn-only, no email)

```html
---
layout: default
title: Contact
cv: passive
contact: active
description: How to get in touch with Ablimit A. Keskin.
---
<div class="contact">
	<p>The best way to reach me is on LinkedIn:</p>
	<ul class="list-unstyled">
		<li><a href="https://www.linkedin.com/in/ablimit/">LinkedIn — /in/ablimit</a></li>
		<li><a href="https://github.com/ablimit">GitHub — @ablimit</a></li>
		<li><a href="https://scholar.google.com/citations?user=P1KTbgkAAAAJ&hl=en">Google Scholar</a></li>
	</ul>
</div>
```

- [ ] **Step 2: Delete the removed pages**

```bash
git rm projects/index.html notes/index.html
```

Expected: git stages both deletions.

- [ ] **Step 3: Verify no in-repo links point to deleted pages**

Run: `grep -rniE "href=\"/(projects|notes)" _includes index.html cv/index.html contact/index.html`
Expected: no output.

- [ ] **Step 4: Commit**

```bash
git add contact/index.html
git commit -m "Contact: LinkedIn-only; remove stale Projects and Notes pages"
```

---

### Task 5: Modernize styling layer (`css/style.css`)

**Files:**
- Modify: `css/style.css` (fix imports; append additive layer at end of file)

**Interfaces:**
- Consumes: markup classes from Tasks 2–4: `.intro`, `.lead`, `.links`, `.cv`, `.cv-subtitle`, `.avatar`, `.header`, `.footer`, `.nav-pills`.
- Produces: nothing downstream.

- [ ] **Step 1: Fix insecure font imports**

In `css/style.css` lines 1–2, change both `http://fonts.googleapis.com` to `https://fonts.googleapis.com`.

- [ ] **Step 2: Append the modern styling layer at the end of `css/style.css`**

```css

/* ============================================================
   2026 refresh — additive layer (Bootstrap untouched)
   ============================================================ */
:root {
	--ink: #1a1a1a;
	--muted: #6b7280;
	--rule: #ececec;
	--accent: #0b6b5f;   /* healthcare teal */
	--accent-hover: #08504a;
}

body {
	color: var(--ink);
	line-height: 1.7;
	-webkit-font-smoothing: antialiased;
}

@media (min-width: 768px) {
	.container { max-width: 820px; }
}

/* Nav / masthead */
.header { border-bottom: 1px solid var(--rule); }
.header h3 a, h3.text-muted a { color: var(--ink); letter-spacing: .04em; }
.header h3 a:hover, h3.text-muted a:hover { color: var(--accent); }
.nav-pills > li > a { color: var(--muted); border-radius: 3px; }
.nav-pills > li > a:hover { color: var(--accent); background: #f5f7f7; }
.nav-pills > li.active > a,
.nav-pills > li.active > a:hover,
.nav-pills > li.active > a:focus { background: var(--accent); color: #fff; }

/* Links */
a { color: var(--accent); }
a:hover, a:focus { color: var(--accent-hover); }

/* Homepage hero */
.intro { margin: 50px 0; align-items: center; }
.intro .avatar { border-radius: 8px; box-shadow: 0 6px 22px rgba(0,0,0,.10); margin-bottom: 20px; }
.intro .lead { font-size: 24px; line-height: 1.35; font-weight: 700; margin-bottom: 18px; }
.intro p { color: #2b2b2b; }
.intro .links { margin-top: 24px; font-family: 'Montserrat', Helvetica, sans-serif; text-transform: uppercase; letter-spacing: .05em; font-size: 13px; }

/* CV refinements */
.cv { width: 100%; max-width: 720px; }
.cv h1 { margin-bottom: 2px; }
.cv .cv-subtitle { font-family: 'Montserrat', Helvetica, sans-serif; text-transform: uppercase; letter-spacing: .06em; color: var(--accent); font-size: 15px; margin-bottom: 26px; }
.cv h2 { margin-top: 40px; padding-bottom: 6px; border-bottom: 1px solid var(--rule); }
.cv ul { margin-bottom: 8px; }
.cv p strong { color: var(--ink); }

/* Contact */
.contact { max-width: 640px; }
.contact ul.list-unstyled li { line-height: 34px; font-size: 17px; }

/* Footer */
.footer { border-top: 1px solid var(--rule); color: var(--muted); }
```

- [ ] **Step 3: Verify Bootstrap files untouched and imports fixed**

Run: `git status --short css/`
Expected: only `css/style.css` modified — no `bootstrap*.css` changes.

Run: `grep -c "http://fonts.googleapis" css/style.css`
Expected: `0`.

- [ ] **Step 4: Visual check via throwaway mock (not committed)**

Create a temporary `/tmp/cvmock.html` that inlines the CV markup from Task 3 and links `css/bootstrap.css` + `css/style.css` via absolute file paths, open it with the Playwright browser tool, screenshot desktop (1280px) and mobile (390px) widths, and confirm: teal accent on headings/links, readable spacing, avatar card shadow, nav pills styled. Delete `/tmp/cvmock.html` afterward. Do NOT add any mock file to the repo.

- [ ] **Step 5: Commit**

```bash
git add css/style.css
git commit -m "Modernize styling: teal accent, refined hero/CV/nav, https font imports"
```

---

### Task 6: Final review and plan/spec housekeeping

**Files:**
- Review only (no new source changes unless a defect is found).

- [ ] **Step 1: Full-repo stale-content sweep**

Run: `grep -rniE "komodo health</span>|@Komodo|research scientist.*present|© ablimit 2018|goodreads|happen to know my email|Download CV" . --include=*.html --include=*.yml`
Expected: no output.

- [ ] **Step 2: Confirm no dangling nav/link targets**

Run: `grep -rniE "href=\"/(projects|notes)\"" . --include=*.html`
Expected: no output.

- [ ] **Step 3: Confirm the three awards and Kouper appear where expected**

Run: `grep -l "Kouper" index.html cv/index.html && grep -c "Best Poster\|Best Presentation\|Test of Time" cv/index.html`
Expected: both files listed; count ≥ 3.

- [ ] **Step 4: Confirm working tree is clean and push branch**

```bash
git status --short
```
Expected: clean.

Then confirm with the user before pushing `update-kouper` (outward-facing — see handoff).
