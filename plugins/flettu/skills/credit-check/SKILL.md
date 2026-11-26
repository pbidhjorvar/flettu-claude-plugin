---
name: credit-check
description: Lay out an Icelandic company's liquidity, solvency and profitability over several years from its filed annual reports with Flettu, and state warning signs as facts with their numbers (negative equity, two loss years, falling equity ratio, late filing). Never a score or recommendation. Use when the user asks for a credit check, counterparty or supplier check, due diligence on figures, or whether a company is in difficulty.
---

# Credit check (facts only)

The filed figures that a credit reader looks at, over five years, with each
warning sign stated as a fact and its number. The reader draws the
conclusion; this skill never does.

## Tool sequence

1. `search_companies`, then `get_company`: status, group, auditor and auditor
   history, `filed_reports`.
2. `get_financials` with the latest five `years` and `metrics`:
   - liquidity: `cash`, `current_assets`, `current_liabilities`,
     `current_ratio`, `operating_cash_flow`
   - solvency: `total_assets`, `total_equity`, `total_liabilities`,
     `long_term_liabilities`, `equity_ratio`
   - profitability: `revenue`, `ebitda`, `operating_profit`,
     `net_financial_items`, `net_profit`, `profit_margin`
   - other: `dividends_paid`, `employees`
3. `get_company_timeline`: status changes and `annual_report_filed` events
   with their filing date.
4. For a year in `checks`, or any figure a warning rests on, call
   `get_report_evidence` with its `document_id` and page and read it first.
5. Optional: `match_customer_list` with this one kennitala gives Flettu's
   standard warnings for the latest year as a cross-check.

## Warning signs (state as facts)

| Sign | When to state it |
|---|---|
| Negative equity | `total_equity` < 0 in a year: "Equity was −84 m.kr. at end of 2025." |
| Two loss years | `net_profit` < 0 in two consecutive years, with both numbers |
| Falling equity ratio | `equity_ratio` lower each year for three years or more, with the series |
| Thin equity | `equity_ratio` below 10 % |
| Current liabilities above current assets | `current_ratio` below 1, with both amounts |
| Revenue down 20 % or more | year-on-year, with both years |
| Negative operating cash flow | the years and amounts |
| Late filing | a report filed more than eight months after its year end (the legal deadline), from the filing date in the timeline; or no accounts for the last two years |
| Auditor changed | the years and names |
| Status | anything other than active in the timeline (for example bankruptcy or winding up) |

If none apply, say "None of the listed signs appear in the years read." Do
not call that good news.

## Deliverable

```
# Credit facts: <Name> (<kennitala>), <years>, <company | group> account

## Liquidity
| | 2021 | ... | 2025 |   (cash, current assets, current liabilities, current ratio, operating cash flow)
## Solvency
| | ... |   (assets, liabilities, equity, equity ratio)
## Profitability
| | ... |   (revenue, EBITDA, operating profit, net profit, margin)
Sources: per year, pdf_url or document and page.

## Warning signs found
- <sign>: <numbers, years> (source)

## Gaps
Missing years, unread reports, failed checks, rounded figures.
```

## Sources and links

- Filed figures carry `source: {document_id, page, pdf_url}`; cite `pdf_url`,
  or name the document and page. Link the company with the `page` or
  `company_url` returned; never build a flettu.is URL.
- Missing is not zero: a missing year is "missing", and no sign is inferred
  from it.

## Hard rules

- No score, rating, grade, probability of default, credit limit,
  recommendation, or words like "healthy", "risky", "safe" or "distressed".
- No figures that are not in the filed reports; no estimates for missing
  years.
- Icelandic number format: `502 m.kr.`, `1,6 ma.kr.`, decimal comma.
