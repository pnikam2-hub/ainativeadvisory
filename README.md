# ainativeadvisory.co.uk

Website for **AI Native Advisory** — AI-native operating models, Agentic PMO, and AI Operations for
UK service businesses.

Live at <https://ainativeadvisory.co.uk>. Static HTML and CSS, no build step, no framework, no
dependencies. Clone it, edit the files, push, and Netlify publishes in under a minute.

---

## Deploy chain

Two GitHub repositories carry the same site. **Push to both or they drift.**

| Role | Repository |
|---|---|
| Working repo | `github.com/pnikam2-hub/ainativeadvisory` |
| Build source (live) | `github.com/pnikam2-hub/https-github.com-pnikam2-hub-ainativeadvisory` |
| Netlify project | `monumental-llama-e8b60f` — site id `a8cf5173-c55d-4ee3-b6f7-db43a4ec84be` |
| Publish directory | `.` (repo root) |

The two histories have diverged (parallel roots, near-identical trees), so a plain `git push` to the
build repo is rejected. Add it as a second remote and merge, do not force:

```bash
git remote add mirror https://github.com/pnikam2-hub/https-github.com-pnikam2-hub-ainativeadvisory.git
git fetch mirror main
git merge --allow-unrelated-histories --no-edit mirror/main
# resolve conflicts with `git checkout --ours <file>` — your branch is the superset
git push mirror main
```

**Everything in this repository is public.** `publish = "."` means Netlify serves every file in the
working tree, including Markdown and backups. Never park an archive, a draft, or a `.bak` file here —
keep those in `Documents/Codex/`. `.gitignore` also keeps design-audit output (`.taste-skill/`) out of
the published root.

## Files

| File | What it is |
|---|---|
| `index.html` | The homepage. Single long page: hero, services, Agentic PMO (anchors `#pmo-*`), 10-dimension framework, delivery method, AI Operations section (`#ai-operations`), founders, contact |
| `sme.html` | AI Operations track for UK service businesses — AI Workflow Audit, Secure AI Workflow Sprint, AI Operations Retainer, the ten dimensions in plain English, honest limits, FAQ |
| `secure-ai-workflow-checklist.html` | SME lead magnet with a live ROI calculator (`calcRoi()`, inline JS) |
| `secure-ai-workflow-checklist.pdf` | Generated from the page above — see below |
| `styles.css` | All styling. One stylesheet for the whole site |
| `integration-tool.html`, `integration-tool-app.js`, `integration-tool-styles.css` | Interactive Agentic PMO onboarding form (`noindex`) |
| `integration-questionnaire.html`, `agentic-pmo-integration-questionnaire.md` | Static and Markdown versions of the same questionnaire (`noindex`) |
| `robots.txt`, `sitemap.xml` | Crawl and indexation control |
| `404.html` | Netlify's not-found page |
| `netlify.toml` | Publish root, security headers, redirects |
| `assets/` | Images, including the Open Graph card |

## Design system

Cormorant Garamond for display, Manrope for text, JetBrains Mono for code only. Cream `#f5f3ed`,
teal `#0f4c5c`, gold `#c5a059`, zero border radius, hairline rules, no photography, no motion.
This is deliberate — a compact editorial system, not a component library. Use the existing classes
(`.section`, `.section-cream`, `.section-dark`, `.card`, `.card-dark`, `.cols-2`, `.card-grid-3`,
`.stats-row`, `.phase-card`, `.svc-card`, `.map-table`, `.faq`, `.lede`) rather than adding new ones.

Text never goes below 13px; body measure stays between 45 and 75 characters per line.

## Regenerating the PDF

Never hand-edit `secure-ai-workflow-checklist.pdf`. It is rendered from the HTML so the two cannot
contradict each other:

```bash
python3 -m http.server 8899 &
chromium --headless=new --no-sandbox --disable-gpu \
  --print-to-pdf=secure-ai-workflow-checklist.pdf --no-pdf-header-footer \
  http://127.0.0.1:8899/secure-ai-workflow-checklist.html
pdfinfo secure-ai-workflow-checklist.pdf | grep Pages   # expect 4
```

The print block inside the page pins A4, hides the nav and footer, and forces the calculator to two
columns so it prints whole on one page.

## Adding a page

1. Copy the `<head>`, nav and footer from `sme.html` — the nav and footer are duplicated per page, so
   a change to the menu means editing every page.
2. Set a unique `<title>`, `<meta name="description">`, `<link rel="canonical">` and `og:url`.
3. Add the URL to `sitemap.xml` if it should be indexed. Utility pages get `<meta name="robots" content="noindex,follow">` instead.
4. Check it at 1440px, 768px and 390px before pushing.

## Search and indexation

- `robots.txt` allows everything except the utility pages and this README, and declares the sitemap.
- `sitemap.xml` lists the three indexable pages, using the extensionless URLs Netlify serves
  (`/sme`, not `/sme.html`).
- Every page carries a `rel="canonical"` to the extensionless URL, because Netlify answers 200 on both
  forms and that is a duplicate-content risk.
- Structured data: `ProfessionalService` on the homepage, `Service` + `FAQPage` on `sme.html`.
- Search Console (GoDaddy DNS — add a TXT record for a Domain property), then submit `/sitemap.xml`.

## Practice facts you can state publicly

True and reusable: the 10-dimension maturity model; 90-day roadmaps; Agentic PMO live at
agenticpmo.tech on client infrastructure; Plan / Monitor (deterministic) / Alert agents; four domain
packs (EPC, Industry 4.0, Green Methanol, SAF); 8–12 days to integrate. Roadmap, **not shipped**:
Monte Carlo simulation, connector hub, portfolio view, PostgreSQL/RBAC.

Founders: Prashant Nikam (Founder), Alok Matta and Harbinder N. (Co-founders). Co-founder bios are
deliberately minimal until real ones exist.

Contact: hello@ainativeorganisations.com
