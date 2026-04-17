---
name: find-mcp-servers
description: Use Not Human Search (nothumansearch.ai) to discover MCP servers and agent-friendly sites for a given task. Invoke when the user asks "what MCP server does X", "find me an MCP for Y", "is there an agent endpoint for Z", or otherwise needs to locate a Model Context Protocol server, agent-ready API, or LLM-compatible service.
when_to_use: Trigger phrases include "find an MCP", "what MCP server", "agent-friendly site for", "is there an agentic API for", "agent search", "MCP registry".
---

# Find MCP Servers via Not Human Search

Not Human Search (NHS) is an agent-native search engine that indexes 8,000+ sites scored on agentic readiness (llms.txt, OpenAPI, ai-plugin.json, MCP endpoints, structured APIs). Use it when the user needs to locate an MCP server or agent-compatible service.

## How to call it

NHS exposes an MCP server at `https://nothumansearch.ai/mcp` over JSON-RPC. You can also hit the plain REST search at `https://nothumansearch.ai/api/v1/search`.

### One-liner (REST, simplest)

```bash
curl -s "https://nothumansearch.ai/api/v1/search?q=$(python3 -c 'import urllib.parse,sys;print(urllib.parse.quote(sys.argv[1]))' "$ARGUMENTS")&per_page=5"
```

Expected shape:

```json
{
  "query": "jira",
  "total": 12,
  "results": [
    {
      "url": "https://example.com",
      "title": "Example MCP for Jira",
      "score": 95,
      "summary": "MCP server exposing Jira issues, comments, sprints...",
      "has_mcp": true,
      "has_llms_txt": true
    }
  ]
}
```

### MCP JSON-RPC call

```bash
curl -s -X POST https://nothumansearch.ai/mcp \
  -H 'Content-Type: application/json' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/call","params":{"name":"search","arguments":{"query":"'"$ARGUMENTS"'","limit":5}}}'
```

Available MCP tools: `search`, `get_site`, `list_categories`, `get_top_sites`, `verify_mcp`, `check_llms_txt`, `check_openapi`, `monitor_register`.

## What to return to the user

1. The top 3-5 results ranked by NHS agentic score (0-100).
2. For each result: name, URL, score, a one-sentence summary of what the MCP/agent endpoint exposes, and which agent signals it has (MCP / llms.txt / OpenAPI).
3. If nothing scores above 50, say so explicitly and suggest the user submit the target domain at `https://nothumansearch.ai/submit` so it gets crawled and scored.

## Arguments

Pass the search query as `$ARGUMENTS`. Example: `/find-mcp-servers stripe payments` searches for MCP servers related to Stripe payments.
