# Applied / Prepared Index — de-dup source of truth

**Purpose:** one line per application packet ever built, so the weekly scan screens for duplicates by reading
**this one file** instead of opening every numbered folder. Append a row the moment a packet is filed. Keep it
in sync — a packet not listed here is invisible to dedup.

**Columns:** `N` = folder number · `Employer` · `Role` · `Status` (applied / prepared) · `Filed` (YYYY-MM) ·
`Fit`. Use `—` when a value wasn't recorded; fill going forward.

| N | Employer | Role | Status | Filed | Fit |
|---|----------|------|--------|-------|-----|
| | | | | | |

<!-- If backfilling from existing application folders, add one row per folder here (employer + role from the
     folder name is enough for dedup; dates/fit can be filled later). -->
