# prospecting

A personal B2B prospecting assistant. The user describes a target in plain language
("B2B SaaS companies selling to retailers, in Portugal") and gets a shortlist
of real companies with evidence and the key people to contact on LinkedIn.

Run the `/prospect` skill for any request of that shape, even if the user doesn't name it.

## Tools: free only
| MCP server | What it does here | Cost |
|---|---|---|
| `exa` (hosted, no key) | `web_search_exa` finds companies (`category:company`) and LinkedIn profiles (`category:people`); `web_fetch_exa` reads pages | free, rate-limited |
| `tavily` (hosted, OAuth login) | `tavily-search` / `tavily-extract`: second search engine, case studies, news, procurement, partner lists | free, 1,000 credits/month |

Works for any country and segment. Company registries are checked through search (the
country's official business register).

Don't add paid tools. Apollo's free plan can't search companies, so it's not used.

## Rules
- Every claim about a company (what it does, that it serves the segment) needs a source URL.
- Never invent or "construct" a LinkedIn URL, email address or name. List only what a search
  result actually showed. If you can't find someone, say "not found".
- No logging into or scraping LinkedIn: only public search results.
- Mark a company as unverified when the registry can't confirm it, and say why.
- Save results to `output/`. That folder isn't committed.
