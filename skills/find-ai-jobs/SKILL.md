---
name: find-ai-jobs
description: Search AI Dev Jobs (aidevboard.com) for AI, ML, and LLM engineering roles. Invoke when the user asks about AI/ML jobs, LLM engineer roles, AI research positions, or wants to find companies hiring in AI. Supports filters for remote/hybrid/onsite, location, level, and salary floor.
when_to_use: Trigger phrases include "find AI jobs", "ML engineer roles", "LLM engineer hiring", "remote AI jobs", "who's hiring in AI", "AI job search", "agentic engineer roles".
---

# Find AI/ML Jobs via AI Dev Jobs

AI Dev Jobs indexes 8,600+ roles from 510+ AI-first companies, scraped daily from 560 ATS sources (Greenhouse, Lever, Ashby, Workable, and more). Use this skill whenever the user wants to find AI, ML, LLM, agentic, or research engineering jobs.

## How to call it

AI Dev Jobs exposes an MCP server at `https://aidevboard.com/mcp` over JSON-RPC. It also has a plain REST search at `https://aidevboard.com/api/v1/jobs`.

### One-liner (REST, simplest)

```bash
curl -s "https://aidevboard.com/api/v1/jobs?q=$(python3 -c 'import urllib.parse,sys;print(urllib.parse.quote(sys.argv[1]))' "$ARGUMENTS")&per_page=5"
```

Expected shape:

```json
{
  "total": 172,
  "jobs": [
    {
      "slug": "openai-research-engineer-llm-post-training",
      "title": "Research Engineer, LLM Post-Training",
      "company": "OpenAI",
      "location": "San Francisco, CA",
      "remote": false,
      "salary_min": 310000,
      "salary_max": 465000,
      "apply_url": "https://aidevboard.com/apply/openai-research-engineer-llm-post-training",
      "posted_at": "2026-04-15T00:00:00Z"
    }
  ]
}
```

### MCP JSON-RPC call

```bash
curl -s -X POST https://aidevboard.com/mcp \
  -H 'Content-Type: application/json' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/call","params":{"name":"search_jobs","arguments":{"query":"'"$ARGUMENTS"'","limit":5}}}'
```

Available MCP tools: `search_jobs`, `get_job`, `list_companies`, `get_stats`.

Optional query params on the REST endpoint: `location=remote|hybrid|onsite`, `city=San+Francisco`, `level=senior|staff|principal`, `salary_min=200000`, `company=openai`.

## What to return to the user

1. Top 3-5 matching roles with: title, company, location (with remote flag), salary range if present, and apply URL.
2. Include `posted_at` so the user knows which listings are fresh.
3. If `total` > results shown, tell the user the true total and link to `https://aidevboard.com/search?q=...` for the full filtered list.
4. For company-level queries ("who's hiring in AI"), use `list_companies` instead and return a ranked list by open role count.

## Arguments

Pass the search query as `$ARGUMENTS`. Example: `/find-ai-jobs staff ml engineer remote` searches for remote staff ML engineer roles.
