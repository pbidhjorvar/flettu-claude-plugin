---
name: ownership-map
description: Map who owns and runs an Icelandic company, and which other companies a recorded owner or board member is linked to, with Flettu. Shows shareholders, beneficial owners, board, managers and auditor with their dates and sources, and never treats two people with the same name as one person. Use when the user asks who owns a company, who sits on its board, or what else a person or holding company is involved in.
---

# Ownership map

Owners and roles around one company, or the companies around one recorded
name, each link dated and sourced.

## Tool sequence

**Starting from a company**
1. `search_companies`, then `get_company`: group, people (board, managers,
   auditor and auditor history), and any disclosed owners such as
   `pension_owners`.
2. For each owner or board member the user wants followed:
   `search_company_relationships` with `name` = the recorded name, `roles` as
   needed (`shareholder`, `beneficial_owner`, `director`, `chairperson`,
   `management_role`, `signatory`, `auditor`, `governance`), and
   `match: "exact"`. Page with `offset` and `next_offset`.
3. `get_company_timeline` for dated changes of board, owners or auditor.

**Starting from a name**
1. `search_company_relationships` with the name (omit `roles` to get every
   recorded role). Use `match: "prefix"` only when the user gives part of a
   name, and say so.
2. `get_company` for the companies the user wants opened.

## Checks

- **Same name, maybe different people.** Matches on a name are separate
  observations, not a verified person. Never merge them into one person, and
  never infer identity from a shared name, town or age. Write "a person
  recorded as Anna Jónsdóttir" and list each link separately.
- **Shareholder is not beneficial owner.** Keep the two apart.
- **Dates.** Give each relationship's date or period as returned; a link
  without a date is "date not recorded".
- **Absence is not proof.** No match does not mean no relationship.

## Deliverable

```
# Ownership and roles: <Name> (<kennitala>)

## Owners
| Owner | Type (shareholder / beneficial owner) | Share | As of | Source |
## Board and management
| Name as recorded | Role | From | To | Source |
## Auditor
<firm or person>, <period>; earlier auditors
## Other companies linked to <name as recorded>
| Company (link) | Role | Period | Source |
Note: links are on the recorded name; the same name may be different people.

## Gaps
Roles or owners not recorded, undated links.
```

A small diagram (owners above, company in the middle, subsidiaries below) is
fine when it helps; label every edge with its role and date.

## Sources and links

- Link companies with the `company_url` or `page` the tools return; never
  build a flettu.is URL. Cite each relationship's source as returned.

## Hard rules

- No personal kennitala, address, birth date or other personal data about
  private individuals; do not ask for them or search for them.
- Facts only: no judgement of owners, conflicts of interest or control
  beyond what the records state.
