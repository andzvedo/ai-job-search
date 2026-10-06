# Hiring Manager / Job Search integration (André Azevedo)

How this fork of [ai-job-search](https://github.com/andzvedo/ai-job-search) pairs with André’s Grok Bot agents. City of record: **Araraquara, São Paulo, Brazil** (confirmed; not Ribeirão Preto).

## Agent map

| Agent / surface | Owns | This repo complements with |
|-----------------|------|----------------------------|
| **Hiring Manager (HM)** | ATS form fill + submit (basics only). Extra essays go via Essays Notion + André’s approval before paste. | Master CV path for volume ATS; `/apply` when tailored PDFs are needed |
| **Essays Notion** | Essay drafts grounded in Notion notes + this repo’s `08-application-forms.md` and profile skills | Canonical profile (`CLAUDE.md`, `.claude/skills/job-application-assistant/01-*.md` … `08-*.md`) |
| **Job Search** | Daily digests of remote PD roles | `/scrape` + `/rank` for multi-portal shortlists and fit scoring |
| **Gmail** | Inbox watch for recruiter replies | `/gmail-sync` + `/outcome` to write status into `job_search_tracker.csv` and `documents/applications/` |

## When to run `/apply` here vs HM master CV

**Run `/apply` in this repo** when André wants role-specific materials:

- Tailored CV + cover letter PDFs (`cv/main_<company>_<role>.tex`, `cover_letters/cover_<company>_<role>.tex`)
- Fit evaluation first (`04-job-evaluation.md`), salary research before pretensão
- ATS keyword alignment and compiled PDF verification checklist

**HM fills ATS with master CV** when:

- Form is basics-only (contact, links, upload one PDF, short yes/no screens)
- Volume / speed matters more than a custom narrative
- Essays are handled separately (Essays Notion → approve → paste)

Standing rule: pretensão is **always researched** (salary tool / HM research dumps) before André decides the number on the form. Never invent rates.

## Standing search rules

- Remote **100%** only (no hybrid, no on-site)
- Brazil-eligible / worldwide / LatAm (contractor, freelance, EOR, or explicit LatAm)
- Pay in **USD** (primary track)
- **No BR employers** on the primary USD track
- Location string on forms: **Araraquara, São Paulo, Brazil**

## Tracker + status

1. Seeded template: `job_search_tracker.example.csv` (committed).
2. Local working copy (gitignored): copy once:

```bash
cp job_search_tracker.example.csv job_search_tracker.csv
```

3. After each HM submit or outcome, update via `/outcome` (or `/gmail-sync` after approval).
4. Dashboard: `/html-report`.

Schema header (do not reorder columns):

```
date,company,sector,role,role_type,channel,status,contact_person,fit_rating,notes,cv_file,cover_letter_file,source
```

Submitted rows use `status=applied`. Hold / pending / paused items keep `status` empty and put the reason in `notes` (e.g. `PENDING: captcha`, `HOLD: essays approval`) so `/outcome` and `/html-report` stay schema-safe.

## Salary seeding from HM research

HM (or this agent) dumps comps into markdown research notes. Seed the gitignored `salary_data.json` from the committed example:

```bash
cp salary_data.example.json salary_data.json
# edit bands as André decides pretensões
python3 salary_lookup.py --validate
python3 salary_lookup.py "Blacksmith"
```

See `docs/salary-research-2026-10.md` and `tools/README_SALARY_TOOL.md`.

## Next tailored `/apply` queue

See [`docs/next-apply-queue.md`](next-apply-queue.md) for Bolder, Tellos, Stellar/SmartyMe, RootstockLabs, and Tether.
