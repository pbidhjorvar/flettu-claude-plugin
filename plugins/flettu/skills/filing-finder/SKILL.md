---
name: filing-finder
description: Find which annual reports an Icelandic company has filed, for which years, when they were filed and whether each is the company's own or the group account, then open and read a page with Flettu. Use when the user asks for a company's ársreikningur, which years it has filed, when a report was filed, or what a given page or note of a report says.
---

# Filing finder

Which reports exist, when each was filed, and the text of a page on request.

## Tool sequence

1. `search_companies`, then `get_company`: `filed_reports` lists each report
   Flettu keeps with `year`, `scope` (`company`, `parent` or `group`) and
   `document_id`, newest first.
2. `get_company_timeline`: `annual_report_filed` events give the filing date
   of each year's report.
3. `get_financials` with the years in question when the user wants the pages
   of the statements: each entry in `filed_reports` then carries
   `statement_pages`.
4. **Read a page:** `get_report_evidence` with `document_id` and `page`. The
   excerpt is at most 4,000 characters; `truncated` marks a page cut short.
   If source access is unavailable, do not try other pages of the same
   document; say which report could not be opened.

## Checks

- **Gaps.** List years missing between the first and latest report. A year
  not listed may not be filed or may not be collected yet; say "not in
  Flettu yet", not "not filed", unless the timeline shows it.
- **Filing date vs year end.** Give both. Icelandic accounts are due eight
  months after the year end; state a later filing date as a fact.
- **Parent and group.** When both exist for a year, list both.
- **Quote, don't paraphrase,** when the user asks what a page says.

## Deliverable

```
# Filed annual reports: <Name> (<kennitala>)

| Year | Account | Fiscal period | Filed on | Report |
|---|---|---|---|---|
| 2025 | group | Jan–Dec 2025 | 2026-05-12 | <pdf_url, or document id> |
| 2025 | parent | ... |
| 2023 | — | — | — | missing |

## Page <n>, <year> report
> quoted excerpt
Source: <pdf_url or document and page>
```

## Sources and links

- Cite each report with its `pdf_url` when returned (the report at that page
  on flettu.is, for signed-in readers), otherwise its document and page.
- Link the company with the `page` or `company_url` returned; never build a
  flettu.is URL.

## Hard rules

- Facts only: no opinion on the quality or timing of the company's
  reporting beyond the dates.
- Never name the origin of a report beyond what the tools return.
