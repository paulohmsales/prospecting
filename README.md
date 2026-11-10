# prospecting

Describe the companies you're looking for in plain language. Claude Code returns a shortlist
of real companies, with evidence and registry checks, plus the key people to contact on LinkedIn.

```
/prospect B2B SaaS companies selling to retailers, in Portugal
```

Results go to `output/` (a Markdown report and a contacts CSV).

## Free tools (MCP servers)
- **Exa** hosted MCP, `https://mcp.exa.ai/mcp`: company and LinkedIn-profile search, page fetch. No key.
- **Tavily** hosted MCP, `https://mcp.tavily.com/mcp/`: second search engine and page extraction. Free login, 1,000 credits/month.

Company registry checks go through search, using each country's official business register.

## Setup
```sh
cd prospecting
claude mcp add --scope project --transport http exa https://mcp.exa.ai/mcp
claude mcp add --scope project --transport http tavily https://mcp.tavily.com/mcp/
claude        # then /mcp → tavily → Authenticate (free account)
```
