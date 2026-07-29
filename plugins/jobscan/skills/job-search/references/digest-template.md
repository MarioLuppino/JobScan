# Weekly digest — output format

Write to `<archive>/Job Search Digests/<YYYY-MM-DD> digest.md`. Two parts: a ranked table, then a per-job
block. Always include the apply link inline in the chat summary too.

## Header

```
# Job Search Digest — <YYYY-MM-DD>

Scanned: <categories/sites covered>. Verification: <tooling used; note any fallback>. Fits at or above the
fit floor: <count>.
```

## Ranked table

| # | Title | Org | Location / Remote | Salary | Posted / Closes | Fit | Tier | Status | Apply |
|---|-------|-----|-------------------|--------|-----------------|-----|------|--------|-------|
| 1 | ...   | ... | ...               | ...    | ...             | 92  | federal | VERIFIED-LIVE | <url> |

`Tier` ∈ {federal, state agency, industry, academic, sales}. `Status` ∈ {VERIFIED-LIVE, UNVERIFIED}.

## Per-job block (one per listing)

```
### <#>. <Title> — <Org>  (Fit <score>, <tier>, <status>)
- **Location / Remote:** ...
- **Salary:** ... (flag if below the preferred floor or requires relocation)
- **Posted / Closes:** ...
- **Why it fits:** one line
- **Top 1–2 matches:** ...
- **Biggest gap:** ... (note if likely fatal)
- **Apply:** <canonical URL>
- **Flags:** sponsorship / location lean / duplicate-of-<folder> / etc., as applicable
```

## Rules

- Candidates only — the digest never creates application folders.
- Never include a listing below the fit floor, even labeled "(reach)".
- Never pad to a target count with an unverified or stale listing; report fewer and say what was searched.
