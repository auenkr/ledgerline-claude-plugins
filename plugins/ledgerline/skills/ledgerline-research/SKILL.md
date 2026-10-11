---
name: ledgerline-research
description: Research Indian listed companies with the Ledgerline MCP server. Use when the user asks about an Indian company's profile, financials, profit and loss, balance sheet, cash flow, ratios, peer comparison, shareholding or ownership, or asks to screen or filter companies by financial criteria. Covers tool order, the symbol format, and how to write a screening query.
---

# Ledgerline company research

Ledgerline exposes read-only company data for Indian listed companies over MCP. The tools are `mcp__plugin_ledgerline_ledgerline__*`. They return reported figures, never live prices, and they never execute trades or payments.

## Authenticate first

The MCP server is remote and uses OAuth. If tool calls fail with a connection or authorization error, tell the user to run `/mcp` in Claude Code and choose **Authenticate** for `plugin:ledgerline:ledgerline`. That opens a browser to sign in to Ledgerline and approve `market:read`. Never ask the user to paste an API key into the chat.

## Order of operations

Always discover before you read. Never guess a symbol.

1. `search_companies` with the user's words. It returns `symbol` values such as `/company/RELIANCE/consolidated/`.
2. Pass the returned `symbol` **unchanged** to every other company tool. The URL itself selects standalone or consolidated data, and it is the only parameter these tools take.
3. `list_companies` (paged, 100 per page) when the user wants to browse rather than search.

If `search_companies` returns nothing relevant, say so. Do not substitute a similar-sounding company.

## Choosing a tool

| The user wants | Tool |
| --- | --- |
| Which companies match financial criteria | `screen_companies` |
| What the company is, where it trades, market cap | `company_profile` |
| A quick financial summary, price data, pros/cons | `basic_financials` |
| Annual reports, announcements, ratings, transcripts | `company_documents` |
| How it compares with peers | `peer_comparison` |
| Profit and loss | `profit_loss` for annual and TTM, `quarterly_profit_loss` for quarters |
| Balance sheet | `balance_sheet` |
| Cash flow | `cash_flow` |
| Margins, returns, valuation ratios | `ratios` |
| Who owns it, and how that changed | `shareholding_pattern`, `shareholding_distribution` |

## Writing a screening query

`screen_companies` takes a single `query` string. It evaluates the whole company universe, so it takes no symbol and does not require a prior search.

- Use the measure labels **exactly** as published. Do not invent, translate, or abbreviate a label. The catalog is at https://stock-market.auenkr.com/documentation#screening and the interactive gallery at https://stock-market.auenkr.com/docs#endpoint=%2Fapi%2Fv1%2Fscreen.
- Supported syntax: `>`, `<`, `>=`, `<=`, `=`, arithmetic `+ - * /`, parentheses, `AND`, `OR`. Keep the expression under 4,000 characters.
- Use `AND` when every condition must match. Wrap mixed logic in parentheses.
- Numeric values use each measure's displayed unit, for example market capitalization in INR crores and percentage measures in percentage points.
- If the user has not given a threshold, comparison, or unit, **ask** before building the expression.

Example: `Market Capitalization > 500 AND Price to Earning < 15 AND Return on capital employed > 22%`

The result contains `query`, `count`, `columns`, and `data`. Each row is keyed by the displayed column labels and includes `Symbol`, which you can pass straight into the company tools for follow-up reads.

## Reporting results

- Quote only what the tool returned, with its period and unit. Say which period a figure belongs to.
- Preserve unavailable values as `null`. Never treat `null` as zero, and never fill a gap with a remembered or estimated number.
- Coverage varies by company and dataset. A dataset with no connected source returns an error; report the actual error instead of describing what the data "probably" says.
- Statement, document and shareholding tools require Personal or Professional access. On a lower plan the call returns `403 PAID_PLAN_REQUIRED`; say that plainly and point to https://stock-market.auenkr.com/pricing.
- On the Hobby plan, company tools are limited to a fixed allowlist of five companies; other companies return `403 FREE_COMPANY_RESTRICTED`.
- Summarize and compare. Do not recommend buying or selling, price a security, or promise a future result. Ledgerline provides financial data and does not give investment advice.
