# Overleaf Trust Center — starter repo (DRAFT)

Starting point for Overleaf's FedRAMP Certification Data Sharing (CDS) build, Obtain date **2027-01-01**. See the companion `fedramp-vdr-ver-handoff-brief.md` for full context on why this exists and what else is in flight (VDR/VER, Secure Configuration Guide publication, etc.).

## What this is

A lean, git-native pattern modeled on [patrick-tarly-co/trust-center](https://github.com/patrick-tarly-co/trust-center): versioned JSON files as the source of truth, a minimal static page to render them, and (eventually) a nightly pipeline that regenerates the JSON from real infrastructure/compliance signals and commits it — so git history becomes the audit trail of Overleaf's posture over time.

This is **not** a finished product. Every file contains a `$comment` or `TODO` field flagging what's real vs. placeholder. Nothing here should be published publicly until reviewed.

## What's real vs. placeholder

**Pulled from the live FedRAMP marketplace listing** (verified 2026-09-23, [fedramp.gov/marketplace/products/FR2414718666](https://www.fedramp.gov/marketplace/products/FR2414718666)):
FedRAMP ID, legal entity name (Digital Science UK Limited), product website, certification type/path/class (Rev5, Agency, Class B), phase (Ongoing Certification), status (FedRAMP Certified as of 2025-01-29), authorization count (3). Re-checked again on 2026-09-23; nothing had changed.

**Pulled from the live public status page** ([status.overleaf.com](https://status.overleaf.com/), fetched 2026-09-23): per-component current status (all operational, no known issues) and the rolling uptime percentage shown for each of the 5 tracked components. See `availability-status.json`'s `$comment` for an important limitation: the status page is a client-rendered incident.io page with no discoverable public API, so day-by-day incident history could **not** be pulled — only the current snapshot and the rolling-window percentage.

**Pulled from an uploaded scan report** (`Scan_Report_August_dgtas-hl_20260917.csv`, a Qualys WAS export covering a scan run 2026-09-06 against `stag-overleaf.com`): summarized in `scan-summary.json`. Result: Low overall risk, 0 vulnerabilities at Levels 1-5, 68 informational-only findings (mostly expected 403/404s from crawling). This is genuinely good news, but it's one scan of one staging target — see the caveats in that file before treating it as broad coverage.

**Everything else is a `TODO` placeholder**, including: UEI number, sales/security contact names, full service description, per-service security-category breakdown, SCG public link (the SCG exists internally per the handoff brief but isn't published yet), documentation overview, next Ongoing Certification Report date, and the current 3PAO/assessor name. Someone with access to the SSP/Certification Package needs to fill these in.

## Known gaps in this draft (be aware before using)

1. **Schema validation not done.** I could not fetch the live schema from `fedramp.gov/schemas/fedramp-certification-package-overview-schema-2026-06-24.json` (repeated timeout) during drafting. `fedramp.json`'s structure is inferred from the CDS-CSO-PUB requirement list and the public Tarly reference repo — **validate it against the real schema before this goes anywhere near production.**
2. **KSI catalog corrected, but applicability is the bigger issue.** `ksi-results.json` now uses the real 10-theme KSI catalog fetched directly from [fedramp.gov/20x/standards/20x-ksi](https://www.fedramp.gov/20x/standards/20x-ksi/) (KSI-CNA, KSI-SVC, KSI-IAM, KSI-MLA, KSI-CMT, KSI-PIY, KSI-TPR, KSI-CED, KSI-RPL, KSI-INR — the earlier draft's "KSI-AFR" theme doesn't appear in the authoritative source and has been dropped). More importantly: **that standard explicitly says it does not apply to FedRAMP Rev 5 authorizations**, and Overleaf's actual certification is Rev5/Class B (the baseline formerly called LI-SaaS). So being on that baseline does not, by itself, mean any KSI is "evaluated" or "met" — there's no official mechanism that checks Overleaf against KSIs today. What's in the file is a self-assessed, informal cross-reference, useful for 20x-readiness visibility, not an official evaluation. Only one theme (KSI-MLA) has any real evidence attached so far, from the scan report above — the rest are still `not-yet-evaluated` and need someone on the security team to confirm actual implementation before flipping status.
3. **No availability-status schema was found published by FedRAMP** for the Class B web-service requirement, so `availability-status-v1.schema.json` is hand-drafted, not copied from an official source.
4. **Availability data has a granularity gap.** `availability-status.json` has real current-status and rolling-uptime data from status.overleaf.com, but not real day-by-day history — see gap #2 above. Someone should either build a small poller against the status page or ask the team running it whether a JSON history export exists.
5. **Access logging (`ACCESS.json`/`ACCESS.md`) is a design sketch, not a built feature** — see `ACCESS.md` for the open architecture questions (access model, what counts as "access," where logging is even feasible for a public static-JSON pattern).
6. **POA&M not yet incorporated.** Dan supplied `FedRAMP-POAM-August26 Overleaf.xlsx`, but the assistant's sandboxed shell was down for this session (a known Windows/mount issue), and reading a binary `.xlsx` isn't possible without it. Once shell access is back, open POA&M items should be extracted and cross-referenced against `scan-summary.json` / `ksi-results.json`. Until then, treat the security posture shown here as incomplete — it doesn't yet reflect any known open POA&M items.
7. **No backend/pipeline exists.** Real KSI evaluation, availability history, and access logging all need actual data sources wired in (Terraform state, monitoring/log queries, TLS scans, status-page polling, an access-gated API layer for the full certification package). This repo only has the data *shape* and mostly placeholder values.
8. **Hosting**: the availability-status page has a hard requirement (Class B) to stay reachable even if Overleaf itself is down — so it can't live on Overleaf's own infrastructure alone. (GitHub Pages, used for this starter, satisfies that independence.)
9. **DS-wide trust center question unresolved** — per the handoff brief, Digital Science is planning a shared trust center across products. This repo is scoped narrowly to Overleaf/FR2414718666 and would need to either become a section within that shared instance or stay a dedicated space alongside it; that decision isn't made here.

## Files

| File | Purpose |
|---|---|
| `fedramp.json` | Certification Package Overview (CDS-CSO-PUB) |
| `ksi-results.json` / `KSI-STATUS.md` | Self-assessed KSI cross-reference, machine + human readable |
| `availability-status.json` / `availability-status-v1.schema.json` | Class B current + rolling availability, sourced from status.overleaf.com |
| `scan-summary.json` | Summary of the most recent Qualys scan report Dan supplied |
| `ACCESS.json` / `ACCESS.md` | Access-log pattern + open design questions (CDS-TRC) |
| `index.html` | Minimal static page rendering the above |

## Viewing locally

`index.html` fetches the JSON files via `fetch()`, which browsers block over `file://` due to CORS. Serve it locally instead, e.g.:

```
cd overleaf-trust-center-starter
python3 -m http.server 8000
```

then open `http://localhost:8000`.

## Suggested next steps

1. Fill in the `TODO` fields in `fedramp.json` from the actual SSP/Certification Package (UEI, contacts, service list, assessor, next OCR date).
2. Validate `fedramp.json` against the real fedramp.gov schema.
3. Open `FedRAMP-POAM-August26 Overleaf.xlsx` and cross-reference its items into `scan-summary.json`/`ksi-results.json` (blocked this round by a sandbox shell outage, not by anything structural).
4. With the security team, confirm which KSI validation items are genuinely implemented today and move those themes from `not-yet-evaluated` to `self-assessed-met` with named evidence — do this deliberately, not in bulk, since KSIs don't formally apply to Overleaf's Rev5 authorization.
5. Build (or ask for) a real data source for day-by-day availability history from status.overleaf.com — current code only has a snapshot + rolling percentage.
6. Decide the access model for `ACCESS.md`'s open questions before building anything.
7. Route this into the DS-wide trust center architecture discussion rather than standing it up in parallel.
