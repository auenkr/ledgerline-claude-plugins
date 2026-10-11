# Ledgerline Claude Code plugins

The official Claude Code plugin marketplace for [Ledgerline](https://stock-market.auenkr.com), an API for Indian company fundamentals, financial statements, ownership and corporate events.

## Install

```shell
claude plugin marketplace add AuenKr/ledgerline-claude-plugins
claude plugin install ledgerline@ledgerline-claude
```

Inside a Claude Code session the same thing is:

```
/plugin marketplace add AuenKr/ledgerline-claude-plugins
/plugin install ledgerline@ledgerline-claude
/reload-plugins
```

Then run `/mcp` and choose **Authenticate** for `plugin:ledgerline:ledgerline`.

## Plugins

| Plugin | What it adds |
| --- | --- |
| [`ledgerline`](./plugins/ledgerline) | The hosted Ledgerline MCP server over Streamable HTTP, plus a `ledgerline-research` skill covering tool order, the company `symbol` format, screening queries and honest reporting of missing data. |

## Not a Claude Code plugin?

Ledgerline also works as:

- a **ChatGPT** connection over MCP — see https://stock-market.auenkr.com/documentation#chatgpt
- a **CLI**, `@auenkr/legerline-cli`
- a **REST API** — see https://stock-market.auenkr.com/docs

## Support

- Documentation: https://stock-market.auenkr.com/documentation
- Support: https://stock-market.auenkr.com/support
- Privacy: https://stock-market.auenkr.com/privacy-policy
- Terms: https://stock-market.auenkr.com/terms-of-service

## License

MIT — see [plugins/ledgerline/LICENSE](./plugins/ledgerline/LICENSE).
