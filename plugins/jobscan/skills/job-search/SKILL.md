---
name: job-search
description: >-
  Weekly job scanner. Searches the web for ACTIVE job listings the user is a strong fit for, verifies each is
  live, scores it against their profile, ranks the top ~10, and writes a dated digest with direct apply links
  — then hands selected jobs to the job-applications skill. Use whenever the user wants to find jobs, run a
  job search, "scan for openings", get a weekly list of matches, or check what's out there. Prepares packets
  for review; never submits. Companion to job-applications.
---

# Job Search (weekly scanner)

This skill finds jobs; **`job-applications`** assesses and drafts. First read the user's compressed profile
digest at **`<jobscan-data>/profile-core.md`** (the data path is set during onboarding; default
`~/.claude/jobscan-data/`). Open the full `profile.md` only when the digest lacks a detail. If no profile
exists yet, tell the user to run **`jobscan-onboarding`** first.

## Hard rules (do not violate)

- **Prepare, never submit.** Produce ready-to-submit packets and direct apply links. The user does the final
  submission. Never auto-submit, fill application forms, or create accounts on any job board / employer ATS —
  read-only browsing only.
- **Active listings only, no fabrication.** Every listing must have a real, working canonical source URL
  actually loaded this run. Never reconstruct a posting from memory or an aggregator snippet. If you can't
  verify it's real and open, leave it out.
- **Two-gate verification.**
  - *Gate 1 (digest):* retrieve each posting; capture the canonical apply URL (employer ATS/careers page, not
    an aggregator), the exact title, and posted/closing date. Tag `VERIFIED-LIVE` or `UNVERIFIED`.
  - *Gate 2 (pre-draft, HARD STOP):* no application material is generated for any job unless its posting is
    re-confirmed live at draft time — no exceptions, however strong the fit.
- **Dynamic portals** (NEOGOV/governmentjobs, Paylocity, USAJOBS, CalCareers, Workday) can't be read by a
  plain fetch. Use **`firecrawl-scrape`** (renders JS) first — a firecrawl load of the real posting confirming
  title + open state counts as `VERIFIED-LIVE`. Fall back to **browser tools** if firecrawl is blocked. If
  neither confirms, leave `UNVERIFIED` and say so. (No Firecrawl key → note it once and use built-in
  fetch/search + browser tools.)
- **No duplicates.** Before finalizing the digest and again at the pre-draft gate, screen every candidate
  against **`<archive>/Applied Index.md`** (the append-only dedup file — read this one file, not every
  folder) plus any do-not-resurface list. Exact/near-exact employer+role match → exclude. Same employer,
  adjacent role → surface once as a possible duplicate and let the user decide.
- **Hard gates (encode the user's from onboarding):** work-authorization/sponsorship logic; salary floor
  (+ higher relocation floor + any government pay-grade floor); location/political-lean handling; the fit
  floor (exclude anything below the chosen score); and the avoid-list (sectors that consistently don't work
  out). Write each as rule + reason + how-to-apply. Always surface salary in the digest.

## Token-efficient staged workflow (STANDING RULE)

Run in distinct stages; treat context as a limited resource. Goal: output indistinguishable from a
full-context workflow at the lowest token cost. Don't narrate intermediate reasoning.

1. **Discovery → structured summary, then discard the posting.** Search, de-dup, drop expired. Distill each
   surviving posting to a compact summary (title, org, location, required/preferred quals, responsibilities,
   skills/tools, certs, **ATS keywords**, research area/industry, seniority, salary, dates, canonical URL +
   verification status). Drop boilerplate/benefits/legal text. **Do this server-side with Firecrawl where
   available** (`firecrawl-agent` structured extraction, or `firecrawl-scrape` + immediate distill) so the
   bulky posting never enters context. Prefer `firecrawl-search` over fetch+search round-trips.
2. **Candidate retrieval — read `profile-core.md` once** per run; reuse for every listing.
3. *(Résumé tailoring and 4. cover letter run in `job-applications` on selection — see that skill.)*

## Where to search

Read `references/sources.md` for boards, API patterns, query templates, and the search-term-coverage +
split-quota rules. Keep the source *categories* (federal, state agency, university/research, non-profit,
industry, transferable-sector); the user's onboarding fills in the field-specific employers and keywords.
Cross-check aggregator hits against the employer's own careers page for the live apply link.

**Preferred tooling (Firecrawl):** `firecrawl-search` for discovery, `firecrawl-map` to find a canonical
posting URL, `firecrawl-scrape` for JS portals. All fall back gracefully to built-in fetch/search/browser
tools; note the fallback in the digest's Process note. `firecrawl-monitor` can watch a few high-yield sources
for new postings between full scans (optional). Never use `firecrawl-interact` (or browser form-fill) to
submit anything.

## Fit scoring

Apply the `job-applications` competency-mapping method. Gate checks (must pass): authorized (or sponsored),
active, meets hard/required quals (or a marked near-miss). Assign a **fit score 0–100** with a one-line
rationale, top 1–2 matches, biggest gap, and a **tier tag** (federal / state agency / industry / academic /
sales) that drives the résumé template. Rank by fit; keep the top ~10 (apply the user's split quota if set).
**Exclude anything below the fit floor** — never pad the count by lowering it. Fewer genuine fits is always
better than a padded list.

## Output: the weekly digest

Write to `<archive>/Job Search Digests/<YYYY-MM-DD> digest.md` using `references/digest-template.md`: a ranked
table + a per-job block. **Include each apply link inline** in the chat summary. The digest lists candidates
only — it does not create folders.

**Digest first, then draft on selection.** Wait for the user to pick jobs. For each pick, file it into the
numbered archive and invoke `job-applications`.

### Filing a selected application

0. **PRE-DRAFT GATE (every time, hard stop):** (a) duplicate-check against `Applied Index.md`; (b) re-load the
   canonical URL and confirm title + employer + open state. If not confirmable live, STOP and report — do not
   create a folder or materials.
1. **Next number:** highest existing numbered folder + 1 (ignore year folders).
2. **Create** `<archive>/<N> <Job Title>/`.
3. **Populate:** `Resume - <role>.docx`, `Cover Letter - <role>.docx`, `Job Posting.md` (downloaded copy),
   `NOTES.txt` (direct apply link + what still needs verifying), and `Outreach Email - <role>.md` only for
   ideal-fit-but-no-sponsorship cases (draft, never sent).
4. **Append one row to `<archive>/Applied Index.md`** (`N | Employer | Role | Status | Filed | Fit`). Missing
   this silently breaks dedup.

## Running as the weekly routine

On schedule or on request: run scan → score → rank → write digest → notify with the top matches + apply
links inline + the digest location. If genuine fits fall short of the target count after a real search
effort, report fewer and say so — never lower the bar.
