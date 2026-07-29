# Where to search — source categories & query rules

Keep the **categories** below; onboarding swaps in your field's specific employers, boards, and keywords.
The point is coverage across *kinds* of employer, not a fixed list.

## Source categories (fill each with your field's specifics)

1. **Federal** — the national government job portal (in the US, USAJOBS; use its API where possible for
   reliable structured search). Filter to the pay-grade floor from onboarding.
2. **State / provincial agencies** — often on NEOGOV / governmentjobs.com-style portals. These need JS
   rendering (Firecrawl or a browser) to read.
3. **University / research institutions** — HR boards, department pages, HigherEdJobs/Chronicle-style
   aggregators.
4. **Non-profit / mission organizations** — sector job boards and org careers pages.
5. **Industry / corporate** — company careers pages (often Workday/Greenhouse/Lever) for the private-sector
   employers in your field.
6. **Transferable-sector** — adjacent industries your fuller work history unlocks (list them from onboarding),
   so the search isn't limited to your single headline identity.
7. **Field society / niche boards** — the professional-society job board(s) for your discipline.

For each category, onboarding records: the boards/URLs, any API pattern, and the target employers.

## Search-term coverage (standing rule)

Run the **full core keyword set** against every site that supports keyword search — not a different subset per
site. Some terms are **asymmetric** (searching one does not surface postings titled with the other), so run
each as its own query. Onboarding lists your asymmetric pairs (e.g. for entomology: "entomology" AND
"entomologist"; for mycology you might need "mycology" / "mycologist" / "plant pathology" separately).

## Split quota (standing rule, if set)

If onboarding set a domestic/international split (e.g. 5 US / 5 international per scan), run the international
branches every scan — not only as a fallback. Apply sponsorship rules per listing. If genuine international
fits fall short after exhausting sources, report fewer and say so; never pad with an unverified listing.

## Query templates

- `"<keyword>" <role-cluster> <location OR remote> after:<recent-date>`
- Employer-direct: `site:<employer-careers-domain> "<keyword>"`
- Federal API: query by keyword + grade floor + location; parse the JSON.

Cross-check every aggregator hit against the **employer's own careers page** for the canonical live apply
link before including it in the digest.
