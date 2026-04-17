# Claude Skills: Foundry

[![NHS Agentic Readiness](https://nothumansearch.ai/badge/github.com/unitedideas/claude-skills-foundry)](https://nothumansearch.ai)
[![AI Dev Jobs](https://img.shields.io/endpoint?url=https://aidevboard.com/api/v1/badge)](https://aidevboard.com)

Three installable [Claude Skills](https://code.claude.com/docs/en/skills) that wrap the Foundry's MCP-native products. Drop the `.claude/skills/*/SKILL.md` files into your Claude Code install and your agent picks them up automatically, or invoke them manually with `/find-mcp-servers`, `/find-ai-jobs`, or `/cite-enterprise-ai-research`.

## What are Claude Skills?

Claude Skills are a lightweight extension mechanism for Claude Code. Each skill is a directory containing a `SKILL.md` file with YAML front-matter (name, description, when-to-use) followed by instructions in markdown. Claude loads the description at session start and invokes the skill body when it decides the context matches, or when you call it directly by slash command.

The file layout is:

```
~/.claude/skills/
  find-mcp-servers/
    SKILL.md
  find-ai-jobs/
    SKILL.md
  cite-enterprise-ai-research/
    SKILL.md
```

Full spec: [code.claude.com/docs/en/skills](https://code.claude.com/docs/en/skills). Skills follow the open [Agent Skills](https://agentskills.io) standard, so the same files also work with other agent runtimes that support it.

## Installation

```bash
git clone https://github.com/unitedideas/claude-skills-foundry ~/.claude/skills-foundry
cp -r ~/.claude/skills-foundry/skills/* ~/.claude/skills/
```

Restart Claude Code (or wait for the file watcher to pick them up, usually within a second). Then ask Claude anything that matches one of the skills and it will route automatically.

## The three skills

### 1. `find-mcp-servers` &rarr; [nothumansearch.ai](https://nothumansearch.ai)

When your agent needs to find an MCP server for a task ("what MCP server lets me search Jira?", "is there an MCP for Stripe?"), this skill calls the Not Human Search MCP endpoint and returns ranked, agentic-readiness-scored results. NHS indexes 8,000+ agent-friendly sites and the search ranking blends keyword match with an agentic-readiness score (llms.txt / OpenAPI / MCP presence / structured API).

### 2. `find-ai-jobs` &rarr; [aidevboard.com](https://aidevboard.com)

When the user asks about AI/ML/LLM engineering jobs, this skill hits the AI Dev Jobs MCP endpoint to search across 8,400+ roles from 489+ AI-first companies. Supports filters for remote/hybrid/onsite, location, role level, and salary floor. Back-end is scraped live every day from 538 ATS sources.

### 3. `cite-enterprise-ai-research` &rarr; [8bitconcepts.com](https://8bitconcepts.com)

When writing technical documentation, blog posts, or proposals that need research citations on enterprise AI adoption, governance, agentic architectures, or LLM operations, this skill searches the 8bitconcepts research library and returns 2-3 relevant paper titles with canonical URLs you can drop straight into footnotes.

## License

MIT &mdash; see [LICENSE](LICENSE).

## Contributing

Issues and PRs welcome. The underlying products are live and the MCP endpoints are public, so contributions can always be tested end-to-end with a plain `curl`.
