# JobScan

A reusable, **field-agnostic** weekly job-scan + tailored-application system for [Claude
Code](https://claude.com/claude-code), packaged as a plugin. It finds live job postings you're a strong fit
for, scores and ranks them into a digest, and — on your selection — drafts ATS-safe tailored résumés and
cover letters. It **prepares packets; it never submits applications.**

Built for research-to-industry transitions (it began as an entomology PhD's job search) but designed to adapt
to any field — mycology, plant pathology, data science, ecology, whatever your profile is. A guided
onboarding skill interviews you and builds your candidate profile, so you don't start from a blank file.

## What's in the box

Two cooperating skills plus onboarding:

- **`job-search`** — the *finder*. Searches boards, de-dups, verifies each posting is live, scores fit, ranks
  a top ~10, writes a dated digest, and stops.
- **`job-applications`** — the *drafter*. Maps an employer's competencies to your evidence and produces a
  tailored résumé + cover letter (`.docx`) and interview prep.
- **`jobscan-onboarding`** — a one-time guided interview that generates your personal `profile.md`, its
  compressed `profile-core.md` digest, per-tier base résumés, a voice file, and an empty applied-index.

It is built around **token-efficiency** (a compressed profile digest, per-tier résumé scaffolds you *edit*
rather than regenerate, a single de-dup index instead of rescanning folders) and **verification discipline**
(no listing is drafted against unless its posting is confirmed live at draft time).

## Install

```bash
# In Claude Code:
/plugin marketplace add MarioLuppino/JobScan
/plugin install jobscan@jobscan
```

Then run onboarding once:

```
Run jobscan onboarding
```

Answer the interview (see [`docs/HANDOFF.md`](docs/HANDOFF.md) Part 1 for the questions). It writes your
personal files to a location you choose (default `~/.claude/jobscan-data/`) and configures your archive path.

After that:

```
Run my weekly job search
```

## Optional but recommended

- **[Firecrawl](https://www.firecrawl.dev/) plugin/API key** — lets the scanner read JavaScript-rendered
  government portals (NEOGOV, Workday, USAJOBS, CalCareers) cheaply and do server-side posting→summary
  extraction. Without it, the system falls back to built-in web fetch/search + browser tools.
- **A Markdown→`.docx` path** — the drafter writes packets in Markdown then renders Word files. Any reliable
  path works (pandoc, the Claude `docx` skill, R's `officer` package, or Word/Docs).
- **A scheduler** — to run the scan weekly unattended.

## Privacy

**This repo ships methodology only.** Your real profile, résumés, digests, and application archive are
generated locally and are git-ignored — they never enter the repository. When adapting or forking, keep it
that way: share the machine, never the career data.

## Adapting to your field

Everything domain-specific lives in your generated `profile.md` and in `references/sources.md` (the boards and
keywords). The skills, formatting rules, verification gates, and filing system are field-agnostic. See
[`docs/HANDOFF.md`](docs/HANDOFF.md) for the full architecture and a step-by-step adaptation guide.

## License

MIT — see [LICENSE](LICENSE).
