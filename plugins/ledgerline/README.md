# Ledgerline for Claude Code

Ledgerline gives Claude read-only access to Indian listed-company data: profiles, basic financials, profit and loss, balance sheets, cash flow, ratios, peer comparisons, shareholding and a company screener.

## What this plugin does

It adds two things to a Claude Code session:

- **One remote MCP server**, `plugin:ledgerline:ledgerline`, pointed at `https://stock-market-mcp.auenkr.com/mcp`.
- **One skill**, `ledgerline-research`, that tells Claude the correct tool order, the `symbol` format, how to write a screening query, and how to report missing data honestly.

## Install

```shell
claude plugin marketplace add AuenKr/ledgerline-claude-plugins
claude plugin install ledgerline@ledgerline-claude
```

Then start Claude Code (or run `/reload-plugins`) and authenticate:

```
/mcp
```

Choose **Authenticate** for `plugin:ledgerline:ledgerline`. A browser opens, you sign in to Ledgerline with Google, and you approve the `market:read` scope. That consent creates a dedicated, revocable connection key, so requests count against the same plan and rate allowance as your REST and CLI usage.

You can revoke access at any time from **Dashboard → API keys** in the Ledgerline dashboard, or by removing the consent from your account.

## Authentication details

The server speaks MCP over Streamable HTTP and advertises RFC 9728 protected-resource metadata at `https://stock-market-mcp.auenkr.com/.well-known/oauth-protected-resource`. It uses OAuth 2.1 with dynamic client registration, so no client id or secret is needed. Access tokens expire after five minutes and are refreshed silently; `offline_access` keeps the connection alive between sessions.

The plugin declares no headers and embeds no credential. Authentication happens entirely through the OAuth flow, so nothing sensitive is stored in this repository or in your settings.

## Tools

| Group | Tools |
| --- | --- |
| Discovery and screening | `search_companies`, `list_companies`, `screen_companies` |
| Company overview | `company_profile`, `basic_financials`, `company_documents` |
| Finance | `peer_comparison`, `quarterly_profit_loss`, `profit_loss`, `balance_sheet`, `cash_flow`, `ratios`, `shareholding_pattern`, `shareholding_distribution` |

Every company tool takes the single `symbol` returned by `search_companies`, passed back unchanged.

## What it sends and fetches

The plugin contains no scripts, no hooks and no executables. When enabled, it makes only MCP requests to `stock-market-mcp.auenkr.com` to run the tool you or Claude invoked. It reads nothing from your machine, starts no background process, and sends nothing to any third party. Full details are in the [Privacy Policy](https://stock-market.auenkr.com/privacy-policy) and [Terms of Service](https://stock-market.auenkr.com/terms-of-service).

## Troubleshooting

- **Server shows as disconnected.** Run `/mcp`, select the server, and authenticate again.
- **`401`.** The consent was revoked or the token expired without refreshing. Re-authenticate in `/mcp`.
- **`403 PAID_PLAN_REQUIRED`.** Detailed statements, documents, ratios and shareholding need Personal or Professional. See [pricing](https://stock-market.auenkr.com/pricing).
- **`403 FREE_COMPANY_RESTRICTED`.** The Hobby plan covers a fixed allowlist of five companies.
- **Tools are missing.** Confirm the plugin is enabled under `/plugin` and that `plugin:ledgerline:ledgerline` is listed in `/mcp`.

## Links

- Documentation: https://stock-market.auenkr.com/documentation
- API reference: https://stock-market.auenkr.com/docs
- Screening guide: https://stock-market.auenkr.com/documentation#screening
- Support: https://stock-market.auenkr.com/support
- Source: https://github.com/AuenKr/ledgerline-claude-plugins
