# Flettu for Claude Code

[Flettu](https://flettu.is) has Icelandic companies' registry facts and the figures
from their filed annual reports, each with its source and page. This plugin
connects Claude Code to Flettu and teaches it how to research a company well:
missing figures are never zero, every figure carries its year, unit and scope,
and sources are cited.

## Install

In Claude Code:

```
/plugin marketplace add pbidhjorvar/flettu-claude-plugin
/plugin install flettu@flettu
```

Then run `/mcp`, choose **flettu** and sign in with your Flettu account in the
browser. No API key is needed. You can revoke the connection at any time on
<https://flettu.is/stjornbord/tengingar>.

## What's inside

- **MCP connection** to `https://flettu.is/api/mcp` (HTTP, OAuth sign-in).
- **Skill `flettu-research`:** when to use which Flettu tool, and how to present
  Icelandic financial figures (periods, groups, units, Icelandic number format).
- **Task skills**, each with its tool sequence, checks and deliverable format:
  - `company-snapshot`: one-page profile with five years of sourced figures.
  - `peer-comparison`: peers by ÍSAT and size, side by side with sources.
  - `credit-check`: liquidity, solvency and profitability with warning signs
    stated as facts. No scores or ratings.
  - `ownership-map`: owners, board and auditor; namesakes kept apart.
  - `customer-list-screen`: a list of kennitölur screened for warnings.
  - `industry-report`: totals, medians and leaders of an ÍSAT industry.
  - `filing-finder`: which reports were filed and when; read a page.

## Try asking

- "What was Festi's revenue in 2025, and on which page of the annual accounts is it?"
- "Compare the equity ratio of Hagar and Festi over the last three years."
- "The ten largest companies in Akureyri by revenue."

Using Claude, ChatGPT or Excel instead? See <https://flettu.is/tengingar>.

Questions: flettu@flettu.is
