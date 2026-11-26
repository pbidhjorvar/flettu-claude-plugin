---
name: customer-list-screen
description: Screen a list of Icelandic customers, suppliers or prospects by kennitala with Flettu's match_customer_list and summarise each company's latest filed accounts and warnings (negative equity, losses, falling revenue, changed auditor, no recent accounts). Use when a sales, credit or procurement team gives a list or spreadsheet of kennitölur or Icelandic company names to check.
---

# Customer list screen

A sales, credit or procurement list in; each company's latest filed accounts
and the warnings Flettu found out, as facts.

## Tool sequence

1. **Get kennitölur only.** Take the kennitala column from the user's list.
   For rows with a name but no kennitala, use `search_companies` and ask the
   user to confirm doubtful matches. Do not send anything else from the list.
2. `match_customer_list` with up to 500 kennitölur per call; split longer
   lists. Personal kennitölur are dropped by the tool; say how many were.
3. Optional, for flagged companies the user wants opened: `get_company`, or
   `query_companies` with `kennitalas` (up to 100) and `limit: 100` for more
   measures in one call.

## Warnings returned

`negative_equity`, `thin_equity` (equity ratio below 10 %), `loss`,
`loss_two_years`, `revenue_down_20pct`, `auditor_changed`,
`no_recent_accounts`. Report each with its numbers and fiscal year.

## Checks

- **Latest year differs** between companies; show each row's fiscal year.
- **Not found** kennitölur are listed separately, never dropped silently.
- **Missing is not zero.** A company with no accounts read is "no accounts
  read", not zero revenue.
- **Groups.** Say when two rows belong to the same group.

## Deliverable

```
# Customer list screen: <n> kennitölur, <date>
Matched <n> · not found <n> · personal kennitölur dropped <n>

## Warnings
| Warning | Companies |
|---|---|
| negative_equity | 4 |
...

## Companies with warnings
| Company (link) | Kennitala | Year | Revenue | Net profit | Equity ratio | Warnings | Source |

## Not found
<kennitölur>
```

Offer the full table as a file (CSV or XLSX) when the list is long.

## Share of wallet

If the user wants share of wallet, they compute it on their side as their
own sales ÷ the customer's revenue. Never ask for, receive into a tool call,
or send the user's own sales figures, prices or contract values to Flettu.

## Sources and links

- Cite each figure's `pdf_url` or document and page when the row carries a
  `source`; link companies with the `page` or `company_url` returned. Never
  build a flettu.is URL.

## Hard rules

- Facts only: no score, rating, tier, credit limit, "approve / decline" or
  "risky" label. Sorting by a warning is fine; ranking by an invented score
  is not.
- Icelandic number format: `502 m.kr.`, `1,6 ma.kr.`, decimal comma.
