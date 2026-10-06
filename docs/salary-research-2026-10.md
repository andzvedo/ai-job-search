# Salary research summary — October 2026

Source dumps (2026-10-06): HM / agent research attached as uploads, then summarized here. Figures are **comps and ask guidance**, not verified offers. Currency: **USD**.

## Blacksmith Agency — Senior PD / Design Systems (remote LatAm)

| Band | Guidance |
|------|----------|
| Competitive ask | **~$30–40/hr** or **~$60–75k/yr** |
| Form placeholder used | **$35/hr or $65k/yr** |
| Employer-posted pay | Unknown on official careers; Chile aggregator bands for this title were lower (~$27–49k/yr) |

Notes: agency client bill rates ($150–199/hr on Clutch) are not designer pay. Nearshore Senior PD Brazil comps (Revelo ~$48–70k median $60k) support the $60–75k ask if they pay USD product-company rates.

## Bolder Apps — UI/UX Product Designer (LatAm contractor)

| Band | Guidance |
|------|----------|
| Best $/hr band (Staff/Senior Brazil, US-client retainer) | **~$40–60/hr** |
| Mid negotiate target | **~$45–55/hr** |
| Employer-posted pay | None on Workable; third-party monthly guesses looked low |

## Stellar Tech / SmartyMe — Product Designer & Builder (remote contractor)

| Band | Guidance |
|------|----------|
| Senior / Staff-leaning Brazil remote contractor context | **~$48–84k/yr** |
| Stellar-specific offer | **Unknown** (not published; ignore RocketHunt title scrapes ~$165k) |
| Negotiation sweet spot (interpolated) | **~$55–70k/yr** if senior + AI-build scope |

## Other pretensões already decided (André)

| Company / role | Decided pretensão |
|----------------|-------------------|
| doola | $50–65k |
| Infinity Labrynth | $60–80k |
| Mavila Consulting | $75/h |
| YO AI Labs / micro1 | $110/h |
| Blacksmith | $35/hr |
| Donorbox | $75k |

## How to seed `salary_data.json`

```bash
cp salary_data.example.json salary_data.json
python3 salary_lookup.py --validate
```

`salary_data.json` stays gitignored. Update categories when André locks a new pretensão. Full method notes: `tools/README_SALARY_TOOL.md`.
