# Excession Labs plugins for Claude Code

One plugin, `excession-security`, with four skills and two remote MCP servers. Both servers are public and read-only. They need no account and no API key.

## Install

In Claude Code:

```
/plugin marketplace add Excession-Labs/claude-plugins
/plugin install excession-security@excession-labs
```

In a shell:

```bash
claude plugin marketplace add Excession-Labs/claude-plugins
claude plugin install excession-security@excession-labs
```

Skills only, for other agents, through the skills.sh installer (https://skills.sh):

```bash
npx skills add Excession-Labs/claude-plugins
```

## Skills

| Skill | What it does |
|---|---|
| `pentest-scoping` | Turns a description of an app into a written pentest scope, based on the PentestCompare scoping checklist |
| `security-headers-review` | Calls `check_security_headers`, explains each failed header and gives nginx, Apache, Caddy and Express snippets |
| `pentest-provider-research` | Compares providers with the PentestCompare tools. Facts come from the dataset, with check dates |
| `agent-approval-gate` | Reviews an AI agent's tools and adds enforced approval for outbound actions, based on the indirect prompt injection lab |

In Claude Code the skills run as `/excession-security:pentest-scoping` and so on. Claude also loads them when a request matches.

## MCP servers

| Name | URL | Tools |
|---|---|---|
| `pentestcompare` | https://pentestcompare.com/mcp | `list_providers`, `get_provider`, `search_site`, `get_page`, `how_to_contact` |
| `penetrationtestservices` | https://penetrationtestservices.com/mcp | `check_security_headers`, `search_site`, `get_page`, `how_to_contact` |

All tools are annotated read-only. `check_security_headers` sends one GET request per redirect to the URL you give it. It reads headers only. It is not a penetration test.

Both servers are in the official MCP registry as `com.pentestcompare/site` and `com.penetrationtestservices/site`.

## Disclosure

Excession Labs publishes pentestcompare.com and also appears in its data as a provider (marked "Publisher-owned"). The provider data is open: https://github.com/Excession-Labs/pentest-provider-dataset

## Sources

- Scoping checklist: https://pentestcompare.com/tools/scoping-checklist/
- Indirect prompt injection lab: https://airedteamingservices.com/research/indirect-prompt-injection-lab/
- Security headers checker: https://penetrationtestservices.com/tools/security-headers-checker/

## Layout

```
.claude-plugin/marketplace.json
plugins/excession-security/
  .claude-plugin/plugin.json
  .mcp.json
  assets/icon.png
  README.md
  skills/<skill-name>/SKILL.md
```

Each `SKILL.md` has `name` and `description` frontmatter, so the skills.sh installer finds them under `plugins/excession-security/skills/`.

## Privacy and support

Privacy policy: https://penetrationtestservices.com/privacy/. Support: support@excessionlabs.ai.

## License

MIT for the plugin code and skills. See `LICENSE`. The provider dataset and site content have their own terms.
