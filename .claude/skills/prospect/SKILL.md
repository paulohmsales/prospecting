---
name: prospect
description: Turn an ICP (ideal customer profile) and a country, or any plain-language target ("B2B SaaS companies selling to retailers, in Portugal"), into a verified shortlist of companies with details and the key people to contact on LinkedIn, saved as a Markdown file. Use for any request to find companies, leads, prospects, accounts or contacts for an ICP.
argument-hint: <ICP or kind of company> in <country> [how many] [who you want to reach]
---

# /prospect

Target: $ARGUMENTS

## 0. Read the request
Pull out these fields, and say in one line how you read them before searching:
- **What they do**: industry or offering (e.g. cybersecurity).
- **Who they serve**: their customer segment (e.g. healthcare: hospitals, regions, medtech, pharma).
- **Where**: country or region.
- **Size**, if given (employees, revenue). **How many**: default 10. **Who to reach**: default roles below.
- **Other ICP criteria**, if given: business model (B2B/B2C, SaaS, services), tech they use,
  growth signals (hiring, funding, expansion), ownership, anything to exclude.
If the user gives an ICP as a description or a pasted block, map it onto these fields.
Ask only if "what they do" or "where" is missing. Otherwise go ahead.

Default roles to find, in order: CEO / founder / managing director; CTO / CISO / head of
technology; head of sales / commercial director / partnerships; whoever leads the segment
(e.g. "healthcare lead", "head of public sector & health"). If the user says what they're
selling, choose the roles that would buy it.

## 1. Find candidates (aim for 2-3x the target count)
Run several searches with different wording, not one:
- Exa `web_search_exa` with `category:company` and a descriptive query, e.g.
  `category:company <offering> firm in <country> with <segment> customers`.
- Exa or Tavily searches for evidence pages: customer cases naming clients in the segment,
  partner directories, industry associations, public procurement wins, conference
  exhibitor lists, news, "top N" lists.
- If the country isn't English-speaking, also search in its main language(s): translate
  the offering and segment terms (e.g. "case study", "customers", the segment's typical
  buyers).
- Include local branches of international firms only if they have a real local entity
  or team. Mark them as "international (local office)".
Keep a running list: name, website, why it's a candidate, source URL.

## 2. Qualify each candidate
Use `web_fetch_exa` or `tavily-extract` on the website (home, about, customers/cases, industries).
Classify the fit with the segment:
- **Strong**: named customers or case studies in the segment, or a dedicated offering for it.
- **Medium**: the segment is listed among the industries they serve, with no named proof.
- **Weak / drop**: no evidence. Drop these unless the list would be too short; then keep them, labelled weak.
Note what they actually sell (services vs product, specialities), size signals and HQ city.
Also check the other ICP criteria (size, model, signals). A company that misses a stated
criterion goes down a fit level, and the card says which criterion it missed.

## 3. Check the company is real and active
- Search the country's official business register through Exa/Tavily (e.g. UK
  Companies House, Germany Handelsregister/Northdata, US state registries/SEC, Brazil CNPJ)
  and cite the register page if one is public.
- If nothing official is found, fall back to signals: recent news, active website, LinkedIn
  company page with employee count. Mark it "registry not checked".
Record the registration number, status (active / dissolved), founded year, employee band,
address and the management names when available (useful as a cross-check for people).
Drop companies that are not active.

## 4. Find the people
For each kept company, search Exa with `category:people`, e.g.
`category:people CEO <Company> <country>`, then `category:people head of sales <Company>`, etc.
Also try Tavily `site:linkedin.com/in "<Company>" <role>`.
For each person record: name, title as shown, LinkedIn URL **exactly as returned**, and the
source. Check it's the right company (same name, same country) and that it's current (title
shows the company now, not "former"). Up to 3-4 people per company.
Never guess a URL, email or name. If you can't find someone, write "not found".

## 5. Output
Write `output/<YYYY-MM-DD>-<short-slug>.md` with:
1. **How the request was read** and the searches run (one line each).
2. **Shortlist table**: # | Company | Website | HQ | Employees | Fit (strong/medium/weak) | Why (one line) | Reg. no.
3. **One card per company**: what they do, segment evidence with source links, registry facts,
   then a people table: Name | Title | LinkedIn | Confidence (high/medium) | Source.
4. **Gaps**: what couldn't be verified, companies dropped and why.
Also write `output/<YYYY-MM-DD>-<short-slug>-contacts.csv` with the columns
`company,website,country,registration_no,fit,person,title,linkedin_url,confidence,source_url`.

In chat, give the shortlist table, the paths to both files, and one line on what to refine
next (e.g. "narrow to companies under 50 employees?").

## Budget
Free tiers are limited. Keep to roughly 40 searches/fetches per run. Don't repeat a query
that has already been run. Prefer Exa (no key); use Tavily for the second opinion and for
pages Exa can't fetch.
