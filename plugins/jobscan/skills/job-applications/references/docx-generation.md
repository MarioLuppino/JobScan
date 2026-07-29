# Generating ATS-safe `.docx` packets

Draft and iterate in Markdown (fast, easy to edit in chat), then render the finalized text to Word. Pick one
path — in recommended order:

## 1. The `docx` skill (recommended default — zero setup)

If the Claude `docx` skill is available (it ships with Claude Code), use it: hand it the finalized Markdown
and ask for a single-column, standard-heading Word file. No local toolchain needed, and it handles headings,
bullets, and page setup. **This is the recommended default for most users.**

## 2. R `officer` (portable, deterministic, good for batches)

If you have R with the `officer` package, this produces clean single-column ATS-safe output and is easy to
script for multiple packets. Reusable helper — save as `build-packet.R` and adapt:

```r
library(officer)

# fp_* define fonts/paragraphs; keep it single-column, black text, standard fonts.
make_resume <- function(sections, out_path, base_size = 11) {
  doc <- read_docx()
  body_par  <- fp_text(font.size = base_size, font.family = "Calibri")
  head_par  <- fp_text(font.size = base_size + 2, bold = TRUE, font.family = "Calibri")
  name_par  <- fp_text(font.size = base_size + 6, bold = TRUE, font.family = "Calibri")

  for (s in sections) {
    if (s$type == "name")    doc <- body_add_fpar(doc, fpar(ftext(s$text, name_par)))
    if (s$type == "contact") doc <- body_add_fpar(doc, fpar(ftext(s$text, body_par)))
    if (s$type == "heading") doc <- body_add_fpar(doc, fpar(ftext(s$text, head_par)),
                                                   fp_p = fp_par(padding.top = 8))
    if (s$type == "body")    doc <- body_add_fpar(doc, fpar(ftext(s$text, body_par)))
    if (s$type == "bullet")  doc <- body_add_fpar(doc, fpar(ftext(paste0("• ", s$text), body_par)),
                                                   fp_p = fp_par(padding.left = 12))
  }
  # US Letter + 1" margins
  doc <- body_set_default_section(doc, prop_section(
    page_size = page_size(width = 8.5, height = 11, orient = "portrait"),
    page_margins = page_mar(top = 1, bottom = 1, left = 1, right = 1)))
  print(doc, target = out_path)
}

# Example:
# make_resume(list(
#   list(type="name", text="Jordan Sample, Ph.D."),
#   list(type="contact", text="Portland, OR · jordan@example.com · 555-0100"),
#   list(type="heading", text="Summary"),
#   list(type="body", text="Plant pathologist ..."),
#   list(type="heading", text="Professional Experience"),
#   list(type="bullet", text="Cultured and identified 4,000+ isolates ...")
# ), out_path = "Resume - Diagnostic Mycologist.docx")
```

Respect any **minimum font size** the user set at onboarding (`base_size`).

## 3. Pandoc (if installed)

`pandoc resume.md -o resume.docx --reference-doc=ats-reference.docx` with a single-column reference template.
Simple, but you must supply an ATS-safe reference `.docx` for consistent styling.

## Whichever path — the ATS rules are the same

Single column · standard headings (Summary, Skills, Professional Experience, Education, Certifications,
Publications) · black text · simple bullets · no tables/text boxes/graphics for parsed content · name and
contact in the body, not headers/footers · submit `.docx` unless the posting requires a text-based PDF.
