# Truesight Documentation

Official documentation for [Truesight](https://truesight.goodeyelabs.com) -- an AI evaluation platform that helps you build expert-grounded evaluations for your AI applications.

## What is Truesight?

Truesight lets you define what "good" and "bad" look like for your specific use case, then deploy those criteria as live API endpoints. Instead of relying on generic metrics, you score AI outputs against your own domain expertise -- in real time, at scale.

**Key capabilities:**

- Upload datasets of AI inputs and outputs (CSV, JSON, JSONL)
- Run Error Analysis to discover evaluation criteria from your data
- Build pass/fail evaluations using Guided Setup -- no coding required
- Deploy evaluations as live REST API endpoints
- Integrate via MCP with Claude, Cursor, VS Code Copilot, and Windsurf
- Review results and promote corrections back into your datasets

## Documentation

| Page | Description |
|------|-------------|
| [Overview](overview.md) | Core concepts and workflow |
| [Getting Started](getting-started.md) | Set up your account and run your first evaluation |
| [Datasets](datasets.md) | Upload and manage evaluation data |
| [Error Analysis](error-analysis.md) | Discover evaluation criteria from your data |
| [Evaluations](evaluations.md) | Build and configure evaluation criteria |
| [Results & Review](results.md) | View results and manage the review queue |
| [Teams & Organizations](teams.md) | Manage members, API keys, and credits |
| [API Integration](api-integration.md) | Score AI outputs via REST API |
| [Platform API Keys](platform-api-keys.md) | Create and manage platform API keys |
| [MCP Integration](mcp-integration.md) | Connect AI assistants via Model Context Protocol |

## Quick links

- **Product**: [truesight.goodeyelabs.com](https://truesight.goodeyelabs.com)
- **API base URL**: `https://api.truesight.goodeyelabs.com`
- **MCP server URL**: `https://api.truesight.goodeyelabs.com/mcp/`
- **Agent skills**: [Goodeye-Labs/truesight-mcp-skills](https://github.com/Goodeye-Labs/truesight-mcp-skills)

## Using with AI assistants

This repository is indexed by [Context7](https://context7.com/Goodeye-Labs/truesight-docs), making Truesight documentation available to AI coding assistants via the Context7 MCP.

For direct MCP integration with Claude, Cursor, VS Code, and Windsurf, see the [MCP Integration](mcp-integration.md) guide.

## License

MIT
