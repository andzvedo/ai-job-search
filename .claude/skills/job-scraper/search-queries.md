# Search Queries for Job Scraper

## Search strategy (André)

**Primary track:** Remote Product Designer roles that accept candidates in Brazil and pay in **USD** (contractor, freelance, EOR, or worldwide-remote employers). English materials.

**Secondary track:** Remote Product Designer roles in **Brazil**. Portuguese materials.

## Installed portal CLIs (primary for `/scrape`)

`/scrape` discovers every portal skill under `.agents/skills/*/SKILL.md` and runs its CLI first. Shipped country-agnostic CLIs include `linkedin-search` and `freehire-search`; any skill added with `/add-portal` is included the same way.

The `site:` query templates below are the **WebSearch fallback** — for portals without a CLI, company career pages, or when a CLI fails.

## Search Sites

Primary:
- **linkedin.com/jobs** — also covered by `linkedin-search` CLI; filter Remote + keywords below
- **freehire / similar** — via `freehire-search` when installed
- **Remote design boards:** remotive.com, weworkremotely.com, remoteok.com, designremotely.co, nodesk.co, dribbble.com/remote-product-design-jobs, wellfound.com / AngelList, ycombinator.com/jobs (design), startup.jobs
- **Brazil:** LinkedIn (Brazil + Remote), Gupy/Kenoby career pages via `site:` when needed, company careers (Nubank, Magalu, etc.)

Secondary (company career pages via Google):
- Direct Google searches with `site:` filters for target companies

## Query Categories

### Priority 1: Remote Product Designer (USD / worldwide — PRIMARY)

```
site:linkedin.com/jobs "Product Designer" Remote
site:linkedin.com/jobs "Senior Product Designer" Remote
site:linkedin.com/jobs "Staff Product Designer" Remote
site:linkedin.com/jobs "Product Designer" contractor OR freelance OR "anywhere" OR worldwide
site:remotive.com "Product Designer"
site:weworkremotely.com "Product Designer"
site:remoteok.com "Product Designer"
site:wellfound.com "Product Designer" remote
```

### Priority 2: AI / Fintech / B2B SaaS domain (PRIMARY)

```
site:linkedin.com/jobs "Product Designer" AI OR "machine learning" Remote
site:linkedin.com/jobs "Product Designer" fintech OR crypto OR "B2B SaaS" Remote
site:linkedin.com/jobs "Product Designer" "design system" Remote
site:linkedin.com/jobs "UX" "Product Designer" "data" Remote
site:ycombinator.com/jobs design
```

### Priority 3: Brazil remote Product Designer (SECONDARY)

```
site:linkedin.com/jobs "Product Designer" Remoto Brasil OR Brazil Remote
site:linkedin.com/jobs "Product Designer" "São Paulo" Remoto
site:linkedin.com/jobs "Designer de Produto" Remoto
site:linkedin.com/jobs "Senior Product Designer" Brasil Remoto
```

### Priority 4: Adjacent / wider net

```
site:linkedin.com/jobs "Founding Designer" Remote
site:linkedin.com/jobs "Design Lead" Product Remote
site:linkedin.com/jobs "UX Designer" "Product Designer" Remote fintech
site:linkedin.com/jobs "Product Designer" "LATAM" OR "Latin America" Remote
```

## Location Filter

When evaluating results:

**Ideal (primary):** Fully remote worldwide, remote-friendly to Brazil, or explicit LATAM / contractor / freelance / EOR; compensation in USD
**Acceptable (primary):** US/EU company hiring international contractors via EOR; async-first
**Acceptable (secondary):** Remote roles in Brazil (any Brazilian city), hybrid only if optional and SP-state feasible — default prefer remote
**Borderline:** "Remote US-only" or "must work US hours exclusively" without Brazil eligibility — verify before applying
**Too far / FAIL:** On-site required, relocation required, citizenship/PR-only with no contractor path, primary track roles that pay only in BRL when seeking USD

## Eligibility keywords to prefer
- Remote worldwide / work from anywhere
- LATAM / Brazil welcome
- Contractor / freelance / 1099 / B2B
- EOR / Deel / Remote.com / Papaya (or similar)

## Eligibility keywords to flag
- Must be based in US/UK/EU
- Citizenship or permanent residency required
- No sponsorship + employee-only (when no contractor option)
- "Brazil not eligible" (historical Zapier-style failures)

## Date Filter

Only include jobs posted within the last 14 days, or with an application deadline that has not yet passed. If a posting date cannot be determined, include it but flag as "date unknown".

## Adapting Queries

If the user specifies a focus area, select queries from the matching category and also generate 2-3 custom queries for that focus. Examples:
- `/scrape AI` → Priority 2 AI queries + custom AI design systems / prototyping queries
- `/scrape Brazil` → Priority 3 only, Portuguese evaluation notes
- `/scrape fintech` → Priority 2 fintech/crypto + compliance-adjacent
