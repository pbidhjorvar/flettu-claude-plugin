---
name: peer-comparison
description: Compare an Icelandic company with its peers using Flettu: pick peers in the same ÍSAT industry and size band, then lay their filed annual-report figures side by side with the source of each. Use when the user asks how a company compares with its competitors, peers or industry, or names two to ten Icelandic companies to compare.
---

# Peer comparison

A side-by-side table of a company and its peers from their filed annual
reports, with a source for every cell.

## Tool sequence

1. `search_companies` for the subject company, then `get_company` for its
   ÍSAT code, group and latest year.
2. **Pick peers** unless the user named them: `query_companies` with
   `year` = the subject's latest complete year, `isat` = its ÍSAT code (use a
   shorter prefix such as `"47"` if fewer than five companies come back),
   `filters` = `{"revenue": {"gte": <half its revenue>, "lte": <double>}}` in
   ISK, `exclude_subsidiaries: true`, `order_by: "revenue"`, `limit: 15`.
   Keep up to nine peers plus the subject. Tell the user how the peers were
   chosen, and let them add or drop names.
3. `compare_companies` with the kennitölur (two to ten), `years` = the last
   three, and `metrics` such as `revenue`, `revenue_change_pct`, `ebitda`,
   `ebitda_margin`, `net_profit`, `profit_margin`, `total_equity`,
   `equity_ratio`, `employees`, `fte`. Read `resolved_metrics` and
   `not_supported` and say what each name became.
4. When the user wants each figure traced to its page, call
   `compare_companies` again with `include_observations: true`, or
   `get_report_evidence` for single figures.

## Checks

- **Same basis.** Do not set a group account beside a company's own account
  without saying so. Say when a peer belongs to a larger group.
- **Fiscal years.** Note peers whose year is not the calendar year.
- **Foreign filers** are converted to ISK at the year's average rate; say so.
- **Missing is not zero.** An empty cell is written "missing", never 0, and is
  left out of any average or median you compute.

## Deliverable

```
# <Subject> and peers, <years>
Peers: ÍSAT <code> <name>, revenue <low>–<high> m.kr. in <year>, groups counted once.

| Company | Revenue <year> | Change % | EBITDA margin | Net profit | Equity ratio | FTE |
|---|---|---|---|---|---|---|
| <Subject> (link) | ... |
| <Peer> (link) | ... |

Sources: one line per company, each figure with its pdf_url or document and page.
Notes: groups, fiscal periods, conversions, missing cells.
```

Position the subject by rank only ("3rd of 8 by revenue"). No judgement.

## Sources and links

- Rows carry `sources: {metric: {document_id, page, pdf_url}}`. Cite the
  `pdf_url`; with none, name the document and page.
- Link each company with the `page` or `company_url` the tools return; never
  build a flettu.is URL yourself.

## Hard rules

- Facts only: no valuation, multiples, rating, score, recommendation, or
  "outperforms / underperforms" verdict.
- Icelandic number format: `502 m.kr.`, `1,6 ma.kr.`, decimal comma.
