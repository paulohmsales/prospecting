# prospecting

Describe the companies you're looking for in plain language. Claude Code returns a shortlist
of real companies, with evidence and registry checks, plus the key people to contact on LinkedIn.

```
/prospect cybersecurity company that serves healthcare clients, in denmark
```

Results go to `output/` (a Markdown report and a contacts CSV).

## Free tools (MCP servers)
- **Exa** hosted MCP, `https://mcp.exa.ai/mcp`: company and LinkedIn-profile search, page fetch. No key.
- **Tavily** hosted MCP, `https://mcp.tavily.com/mcp/`: second search engine and page extraction. Free login, 1,000 credits/month.
- **Nordic Registry** MCP, optional ([source](https://github.com/olgasafonova/nordic-registry-mcp-server)): official
  company registries for Denmark, Norway and Finland, plus Sweden with free credentials. Local binary in `bin/`.
  For any other country, the official register is checked through search.

## Setup
```sh
cd prospecting
GOBIN=$PWD/bin go install github.com/olgasafonova/nordic-registry-mcp-server@v1.2.0
claude mcp add --scope project --transport http exa https://mcp.exa.ai/mcp
claude mcp add --scope project --transport http tavily https://mcp.tavily.com/mcp/
claude mcp add --scope project nordic-registry -- "$PWD/bin/nordic-registry-mcp-server"
claude        # then /mcp → tavily → Authenticate (free account)
```
