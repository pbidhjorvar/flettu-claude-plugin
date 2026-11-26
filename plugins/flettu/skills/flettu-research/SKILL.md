---
name: flettu-research
description: Research Icelandic companies with Flettu: find a company by name or kennitala, read its annual-report figures with their sources, compare companies and rank or screen industries. Use when the user asks about an Icelandic company, its revenue, profit, equity, group or industry, mentions a kennitala, ársreikningur, ehf. or hf., or asks to compare or list Icelandic companies.
---

# Flettu: Icelandic company research

Flettu (flettu.is) holds Icelandic companies' registry facts and figures from
their filed annual reports, with the source and period of each figure. The
first call opens a browser to sign in with a Flettu account (/mcp, then flettu).
No API key is needed.

## The workflow

1. **Lists, rankings and totals across companies:** `query_companies`. The
   largest by revenue, most profitable, most employees, fastest growth, a town,
   an industry (ÍSAT), a legal form or a founding decade; filters on any
   figure; totals per industry, town or legal form (a group counts once).
2. **One company:** `search_companies` with a name or kennitala, then
   `get_company` (profile, group, key figures) and `get_financials` (every
   figure by year with the change from the year before). If several companies
   fit a name, ask the user which one.
3. **Compare:** `compare_companies` (two to ten companies, chosen years).
4. **Show the source.** For a figure read from a filed report, call
   `get_report_evidence` and cite the document and page.
5. **Export:** `export_research` with `format` xlsx, pdf, csv or json when the
   user wants a file.

## How to present results

- **Financial years.** Some companies' years do not follow the calendar (Hagar:
  March to February). A year counts as the calendar year most of its months
  fall in; give the real period when Flettu returns one.
- **Groups.** Say when a company belongs to a larger group, especially when a
  parent and its subsidiary both appear.
- **Unusual figures.** When a row is flagged `unusual`, say the profit likely
  includes one-off items.
- **Missing is not zero.** When a figure is missing, say so. Never treat it as
  0 or estimate it.
- **Say what the number is.** Give the year, unit and scope (company or group)
  with every figure.
- **Icelandic formatting.** Millions of krónur as `502 m.kr.`, billions as
  `1,6 ma.kr.`; decimal comma, full stop for thousands (`13.196`). Kennitala
  as `541205-1520`.
- **Link** each company with the `page` or `company_url` the tools return.
  Never build a flettu.is URL yourself.
- **Cite** a figure's `source.pdf_url` (rows from `query_companies` and
  `compare_companies` carry `sources: {metric: {...}}`), or name the document
  and page.
- **People.** Flettu does not give personal data about private individuals.

## When the data is not there yet

If a company or year has not been collected yet, say the figures are not
available yet and offer to look again later.
