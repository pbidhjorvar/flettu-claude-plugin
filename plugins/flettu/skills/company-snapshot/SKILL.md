---
name: company-snapshot
description: Write a one-page profile of one Icelandic company with Flettu: identity, group, five years of key figures from its filed annual reports with the source page of each, board and auditor, and recent dated events. Use when the user asks for a profile, overview, snapshot, fact sheet or "tell me about" an Icelandic company, an ehf. or hf., or a kennitala.
---

# Company snapshot

A one-page, facts-only profile of one company. Every figure carries its year,
unit, scope and source.

## Tool sequence

1. `search_companies` with the name or kennitala. If several companies fit,
   ask the user which one before going on.
2. `get_company` with the kennitala: identity, status, legal form, ÍSAT
   industry, town, group, people (board, managers, auditor and auditor
   history), `key_figures` and `filed_reports`.
3. `get_financials` with the kennitala, `years` = the latest five and
   `metrics` = `revenue`, `ebitda`, `operating_profit`, `net_profit`,
   `total_assets`, `total_equity`, `equity_ratio`, `operating_cash_flow`,
   `employees`, `fte`. Keep `detail` at `summary`.
4. `get_company_timeline` with the kennitala: status, age and dated events.
5. For any year listed in `checks`, call `get_report_evidence` with that
   year's `document_id` from `filed_reports` and a page from its
   `statement_pages`, and read the figure on the page before using it.

## Checks

- **Scope.** Say whether each figure is the company's own account or the
  group's. A company's own account beside a group account is `parent`.
- **Fiscal year.** When `fiscal_periods` lists a year, give the real period
  (for example March 2024 to February 2025).
- **Rounded figures.** A figure with `rounded: true` is a register summary,
  not the filed report's exact figure; label it so.
- **Unusual.** A year flagged `unusual` likely includes one-off items; say so.
- **Missing is not zero.** A year or figure without a value is written
  "missing", and `unread_years` are years whose report has not been read yet.

## Deliverable

```
# <Name> (<kennitala 000000-0000>)
<company page link>

## Identity
Legal form · status · ÍSAT code and name · town · founded / age
Group: <parent or subsidiaries as returned; "not recorded" if none>

## Key figures (ISK, <company | group>)
| Metric | 2021 | 2022 | 2023 | 2024 | 2025 |
|---|---|---|---|---|---|
Sources: 2025 <pdf_url or document, p. N>; 2024 ...

## Board and auditor
Chair, board members, managing director, auditor (with the year it changed)

## Recent events
- <date> <event> (<date basis>, source)

## Gaps
Missing years or figures, checks that failed, reports not yet read.
```

## Sources and links

- A figure from a filed report carries `source: {document_id, page, pdf_url}`
  (rows from `query_companies` and `compare_companies` carry
  `sources: {metric: {...}}`). Cite `pdf_url`: the report open at that page
  on flettu.is, for signed-in readers. With no `pdf_url`, name the document
  and page.
- Link the company with the `page` or `company_url` the tools return. Never
  build a flettu.is URL yourself.

## Hard rules

- Facts only. No valuation, rating, score, recommendation, or verdict such
  as "healthy" or "weak". Describe changes with their numbers.
- People: report recorded roles only. Flettu gives no personal data about
  private individuals.
- Icelandic number format: `502 m.kr.`, `1,6 ma.kr.`, decimal comma, full stop
  for thousands.
