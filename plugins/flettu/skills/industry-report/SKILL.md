---
name: industry-report
description: Write a facts-only report on an Icelandic industry with Flettu: the largest companies in an ÍSAT industry, totals and medians for a fiscal year, change from the year before, and how many companies the figures cover. Use when the user asks about an Icelandic industry or sector (fishing, retail, construction, software, tourism), its size, its largest or most profitable companies, or industry medians.
---

# Industry report

Size, leaders and medians of one industry for one year, from filed annual
reports, with the coverage stated.

## Tool sequence

1. **Find the ÍSAT code.** Use the code the user gives, or a known prefix
   (`"03"` fishing, `"47"` retail, `"41"`–`"43"` construction, `"62"`
   software). If unsure, `get_company_taxonomy` with `scheme: "ISAT"` and
   `version: "2008"` lists the codes, or use the ÍSAT of a known company from
   `get_company`.
2. **Totals and medians:** `query_companies` with `isat`, `year`,
   `group_by: "isat"` (or `"isat_division"`), `measures` such as `revenue`,
   `ebitda`, `net_profit`, `total_equity`, `employees`, `fte`, and
   `aggregate: "sum"`; again with `aggregate: "median"` and
   `aggregate: "count"`. Groups count once.
3. **Leaders:** `query_companies` with `isat`, `year`, `order_by: "revenue"`
   (and again by `net_profit` if asked), `limit: 10`,
   `exclude_subsidiaries: true`.
4. **Change:** repeat step 2 for the previous year.
5. `get_metric_catalog` if the user asks how a measure is defined.

## Checks

- **Coverage.** State how many companies have a figure for each measure, from
  the `count` and coverage fields. Industry totals cover only companies whose
  reports are read; say so.
- **Year.** Use the latest complete year unless asked; reports arrive up to
  a year after the year ends.
- **Medians are not additive.** Never add medians across groups; request the
  median at the grouping you show.
- **Unusual rows.** Note leaders flagged `unusual` (one-off items).
- **Missing is not zero.** Companies without a figure are left out, and the
  report says how many.

## Deliverable

```
# <Industry> (ÍSAT <code>), <year>

## Size
| Measure | Total | Median | Companies with a figure | Change from <year-1> |

## Largest by revenue
| # | Company (link) | Revenue | Net profit | Equity ratio | FTE |
Sources: per company, pdf_url or document and page.

## Coverage and caveats
Companies counted, groups counted once, missing figures, conversions,
non-calendar fiscal years.
```

## Sources and links

- Rows carry `sources: {metric: {document_id, page, pdf_url}}`; cite the
  `pdf_url`, or name the document and page. Totals and medians cite "sum /
  median of <n> companies' filed figures".
- Link companies with the `page` returned; never build a flettu.is URL.

## Hard rules

- Facts only: no outlook, forecast, attractiveness, rating or advice.
- Icelandic number format: `502 m.kr.`, `1,6 ma.kr.`, decimal comma.
