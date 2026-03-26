# MCP Integration

Model Context Protocol (MCP) lets AI assistants like Claude.ai, ChatGPT, Claude Code, Cursor, Windsurf, and VS Code (with GitHub Copilot) use Truesight as a tool provider. Instead of writing API calls yourself, your AI assistant can manage datasets, build and deploy evaluations, run live scoring, review results, and perform error analysis, all through natural language.

## Overview

Truesight exposes a curated set of tools through MCP. When connected, your AI assistant can:

- List, inspect, create, and upload datasets
- Discover pre-built templates and provision private template datasets
- Configure dataset inputs and judgment criteria
- Create and deploy live evaluation endpoints
- Run live evaluations and score inputs on demand
- View evaluation results
- Browse, flag, and judge review queue items; promote reviewed data back to datasets
- Perform error analysis: suggest error notes, consolidate categories, and apply mappings

## Quick setup

To connect any MCP client to Truesight, you just need the server URL:

```
https://api.truesight.goodeyelabs.com/mcp/
```

Add this URL to your MCP client (see the client-specific steps below), then sign in with your Truesight account when prompted. All tools are available based on your account permissions.

> **Want more control over permissions?** You can also connect using a [Platform API Key](/docs/platform-api-keys) instead. See [Connecting with an API key](#connecting-with-an-api-key) at the bottom of this page.

## Client setup

### Claude.ai and Claude Desktop

Claude.ai and Claude Desktop share the same connectors, so you only need to set this up once.

1. Go to [**Customize > Connectors**](https://claude.ai/settings/connectors?modal=add-custom-connector)
2. Click **Add custom connector**
3. Enter:
   - **Name:** Truesight
   - **URL:** `https://api.truesight.goodeyelabs.com/mcp/`
4. Click **Add**
5. When prompted, sign in with your Truesight account to authorize access
6. Enable the connector in any conversation via the **+** button

### ChatGPT

Requires ChatGPT Pro, Team, Enterprise, or Edu. Developer Mode must be enabled, which shows an orange border around the chat and disables memory.

1. Go to [**Settings > Apps > Advanced settings**](https://chatgpt.com/#settings/Connectors/Advanced) and enable **Developer Mode**
2. Click **Create app**
3. Enter:
   - **Name:** Truesight
   - **MCP Server URL:** `https://api.truesight.goodeyelabs.com/mcp/`
   - **Authentication:** OAuth
4. Check the confirmation box and click **Create**
5. When prompted, sign in with your Truesight account to authorize access

> **Note:** Developer Mode must stay enabled to use custom connectors in ChatGPT. Conversations with custom connectors show an orange "Developer Mode" indicator.

### Cursor

[![Add to Cursor](https://cursor.com/deeplink/mcp-install-dark.png)](cursor://anysphere.cursor-deeplink/mcp/install?name=truesight&config=eyJ1cmwiOiJodHRwczovL2FwaS50cnVlc2lnaHQuZ29vZGV5ZWxhYnMuY29tL21jcC8ifQ==)

Click the button to install with one click, then sign in with your Truesight account when prompted.

Alternatively, add it manually to your project's `.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "truesight": {
      "url": "https://api.truesight.goodeyelabs.com/mcp/"
    }
  }
}
```

Restart Cursor after adding the configuration.

### Claude Code

Add the server with one command:

```bash
claude mcp add --transport http truesight \
  https://api.truesight.goodeyelabs.com/mcp/
```

This adds the server to your user-level config (available in all projects). To scope it to a specific project instead, add `--scope project` before the server name. This writes the config to `.mcp.json` in your project root.

### VS Code (GitHub Copilot)

[![Install in VS Code](https://img.shields.io/badge/VS_Code-Install_Server-0098FF?style=flat-square&logo=visualstudiocode)](vscode:mcp/install?%7B%22name%22%3A%20%22truesight%22%2C%20%22type%22%3A%20%22http%22%2C%20%22url%22%3A%20%22https%3A//api.truesight.goodeyelabs.com/mcp/%22%7D) [![Install in VS Code Insiders](https://img.shields.io/badge/VS_Code_Insiders-Install_Server-24bfa5?style=flat-square&logo=visualstudiocode)](vscode-insiders:mcp/install?%7B%22name%22%3A%20%22truesight%22%2C%20%22type%22%3A%20%22http%22%2C%20%22url%22%3A%20%22https%3A//api.truesight.goodeyelabs.com/mcp/%22%7D)

Click a button to install, then sign in with your Truesight account when prompted. Requires VS Code 1.99+ with GitHub Copilot enabled.

Alternatively, add it manually:

1. Open VS Code and press **Cmd/Ctrl + Shift + P** to open the Command Palette
2. Run **MCP: Add Server**
3. Select **HTTP** as the server type
4. Enter the URL: `https://api.truesight.goodeyelabs.com/mcp/`
5. Give it the name `truesight`

Or add it directly to your `.vscode/settings.json`:

```json
{
  "mcp": {
    "servers": {
      "truesight": {
        "url": "https://api.truesight.goodeyelabs.com/mcp/"
      }
    }
  }
}
```

### Windsurf

1. Open Windsurf and press **Cmd/Ctrl + ,** to open Settings
2. Search for **MCP** or navigate to **Cascade** > **MCP Servers** > **View raw config**
3. Add the Truesight server to the `mcpServers` object:

```json
{
  "mcpServers": {
    "truesight": {
      "serverUrl": "https://api.truesight.goodeyelabs.com/mcp/",
      "disabled": false
    }
  }
}
```

4. Save and click **Refresh** (or restart Windsurf)
5. Sign in with your Truesight account when prompted

## Workflow prompts

The Truesight MCP exposes built-in prompts for clients that support the MCP Prompts primitive (such as Claude Desktop). These provide step-by-step workflow guidance without requiring any additional setup.

| Prompt | Description |
|--------|-------------|
| `truesight_mcp_guide` | Router prompt that maps user intent to the correct workflow prompt. |
| `evaluate_trace_workflow` | Evaluate one or more traces against an existing live evaluation. |
| `error_analysis_workflow` | Analyze dataset traces, annotate error categories, and consolidate failure modes. |
| `review_and_promote_traces_workflow` | Judge review queue items and promote judged outputs back to datasets. |
| `bootstrap_template_evaluation_workflow` | Provision from a pre-built template and deploy quickly. |
| `create_evaluation_workflow` | Full create-evaluation workflow for scoping, deployment, cURL generation, and verification. |

For clients that do not support MCP Prompts, use the companion agent skills instead. See [Agent Skills](#agent-skills).

## Available tools

Each tool requires a specific scope. If you signed in with your Truesight account, all tools are available. If you are using an API key, the key must include the scope listed below. If a required scope is missing, the tool returns an error.

| Tool | Description | Required scope |
|------|-------------|----------------|
| `list_datasets` | List all datasets you have access to | `datasets:read` |
| `get_dataset` | Get details for a specific dataset | `datasets:read` |
| `get_dataset_rows` | Get rows from a dataset with pagination | `datasets:read` |
| `create_dataset` | Create a new empty dataset with specified columns | `datasets:write` |
| `upload_dataset` | Create a dataset and populate it with rows in one step | `datasets:write` |
| `list_templates` | List available pre-built evaluation templates | `datasets:read` |
| `provision_template` | Create a private dataset copy from a template slug | `datasets:write` |
| `configure_dataset_inputs` | Set which column(s) contain the AI input to evaluate | `datasets:write` |
| `configure_judgment_columns` | Configure how the evaluation judges AI outputs | `datasets:write` |
| `update_dataset_row` | Update one or more column values in a single dataset row | `datasets:write` |
| `list_evaluations` | List all evaluations you have access to | `evaluations:read` |
| `get_evaluation` | Get evaluation details including config | `evaluations:read` |
| `deploy_live_evaluation` | Deploy an existing evaluation as a live scoring endpoint | `live-evaluations:write` |
| `create_and_deploy_evaluation` | Create and immediately deploy an evaluation in one step | `evaluations:write`, `live-evaluations:write` |
| `run_eval` | Score inputs against a deployed live evaluation | `live-evaluations:execute` |
| `list_live_evaluations` | List deployed live evaluations | `live-evaluations:read` |
| `list_results` | List evaluation run results | `results:read` |
| `get_result` | Get detailed results for an evaluation run | `results:read` |
| `list_review_items` | List flagged review items | `review:read` |
| `flag_review_item` | Flag an evaluation run's results for human review | `review:write` |
| `judge_review_item` | Submit a human judgment for a single review item | `review:write` |
| `add_reviewed_items_to_dataset` | Add all judged review items as new rows in the linked dataset | `review:write` |
| `suggest_error_notes` | Get AI-suggested error notes and category for a trace | `error-analysis:execute` |
| `consolidate_error_categories` | Get AI suggestions for merging similar error categories | `error-analysis:execute` |
| `apply_category_mappings` | Apply error category consolidation mappings to all matching rows | `datasets:write` |

Each of `create_dataset`, `upload_dataset`, and `configure_dataset_inputs` accepts an optional `media_url_column` for multimodal evaluation. Three input configurations are supported:

- **Text-only**: set `input_columns` only (default).
- **Image-only**: set `media_url_column` and pass `input_columns` as an empty list `[]`. At `run_eval` time, pass `inputs` as `{}` and provide the image via `media_url`.
- **Both**: set `input_columns` and `media_url_column` to distinct columns.

A column cannot appear in both `input_columns` and `media_url_column`. If you omit `media_url_column` from any of these calls, the current value is preserved. To clear it, send `null` through the underlying dataset column-roles API.

### Pagination

All list tools accept optional `page` and `page_size` parameters. Responses include `total`, `page`, `page_size`, and `total_pages` fields alongside the `items` array. Defaults vary by tool:

| Tool | Default `page_size` | Range |
|------|---------------------|-------|
| `list_datasets` | 25 | 1-500 |
| `get_dataset_rows` | 25 | 5-10,000 |
| `list_evaluations` | 25 | 1-500 |
| `list_live_evaluations` | 25 | 1-500 |
| `list_results` | 10 | 1-500 |
| `list_review_items` | 50 | 1-500 |

### Idempotency

The following mutating tools accept an optional `idempotency_key` parameter for safe retries: `create_dataset`, `upload_dataset`, `deploy_live_evaluation`, `create_and_deploy_evaluation`, and `add_reviewed_items_to_dataset`. If you provide the same idempotency key within a 10-minute window, the server returns the cached response instead of repeating the operation.

## Agent skills

For clients that support agent skills (Claude Code, Cursor, and others), companion skills are available that give your AI assistant step-by-step workflow guidance for all Truesight MCP workflows.

Install all skills with:

```bash
BASE=https://raw.githubusercontent.com/Goodeye-Labs/truesight-mcp-skills/main/skills
for skill in truesight-workflows evaluate-trace error-analysis generate-synthetic-data review-and-promote-traces bootstrap-template-evaluation create-evaluation eval-audit build-review-interface; do
  curl -fsSL "$BASE/$skill/SKILL.md" -o ".claude/skills/$skill/SKILL.md" --create-dirs
done
```

Or install globally (available in all projects):

```bash
BASE=https://raw.githubusercontent.com/Goodeye-Labs/truesight-mcp-skills/main/skills
for skill in truesight-workflows evaluate-trace error-analysis generate-synthetic-data review-and-promote-traces bootstrap-template-evaluation create-evaluation eval-audit build-review-interface; do
  curl -fsSL "$BASE/$skill/SKILL.md" -o "$HOME/.claude/skills/$skill/SKILL.md" --create-dirs
done
```

The skills are hosted at [github.com/Goodeye-Labs/truesight-mcp-skills](https://github.com/Goodeye-Labs/truesight-mcp-skills).

## Example interactions

Once connected, you can ask your AI assistant things like:

- "List my Truesight datasets"
- "Show me the rows in dataset ds_abc123"
- "Create a dataset from this CSV data and set up an evaluation for response quality"
- "Run an evaluation on this text using my customer_support live evaluation"
- "What are the latest evaluation results?"
- "Flag evaluation run run_abc123 for review"
- "Analyze the errors in my dataset and suggest categories"
- "Consolidate the similar error categories in my dataset"

The assistant will use the appropriate Truesight MCP tools to fulfill your request.

## Troubleshooting

### "Authentication required" or 401 errors

- Try disconnecting and reconnecting Truesight in your client settings
- Make sure you are signing in with the same Truesight account that has access to your organization's resources
- If using an API key, verify the key is correct, has not been revoked, and the `Authorization` header is formatted correctly: `Bearer ts_pat_...`

### "Missing required scope" errors

If you signed in with your Truesight account, all tools are available. If you see this error, your account may not have access to the resource. Check your organization membership and permissions.

If you are using an API key, the tool requires a scope your key does not have. Create a new key with the needed scopes, or revoke and recreate with additional scopes.

### Tools not appearing

- Restart your MCP client after adding the configuration
- Verify the MCP endpoint URL is correct: `https://api.truesight.goodeyelabs.com/mcp/`
- In Windsurf, make sure the server is not set to `"disabled": true`
- In VS Code, verify GitHub Copilot agent mode is enabled (requires VS Code 1.99+)
- Check your MCP client's logs for connection errors

### "Subscription inactive" or 402 errors

- Your organization's subscription may need attention
- Check your billing status in the Truesight web app

## Security notes

- MCP connections use the same authentication and access control as the REST API
- Your AI assistant can only access resources you have permission to view or modify
- To revoke access, disconnect Truesight in your client settings (e.g., Customize > Connectors in Claude.ai)
- If using an API key, revoke it from [Settings](/app/settings) to immediately disconnect all clients using that key

## Connecting with an API key

If you prefer fine-grained control over which tools your AI assistant can use, you can connect with a [Platform API Key](/docs/platform-api-keys) instead of signing in. The scopes you assign to the key determine which tools are available.

### Step 1: Create a platform API key

1. Go to [Settings](/app/settings) in Truesight
2. Click **Create Key** in the Truesight API Keys section
3. Enter a name (e.g., "Claude", "Cursor", or "VS Code")
4. Select the scopes you want the assistant to have access to (or select all)
5. Click **Create**

### Step 2: Copy the MCP config

After creating the key, the key reveal dialog shows a collapsible **Use with MCP** section. Expand it and click **Copy** to copy the MCP configuration JSON to your clipboard.

The config looks like this:

```json
{
  "mcpServers": {
    "truesight": {
      "url": "https://api.truesight.goodeyelabs.com/mcp/",
      "headers": {
        "Authorization": "Bearer YOUR_API_KEY_HERE"
      }
    }
  }
}
```

### Step 3: Add to your MCP client

Paste the configuration into your MCP client's config file. Client-specific details:

**Cursor:**

[![Add to Cursor](https://cursor.com/deeplink/mcp-install-dark.png)](cursor://anysphere.cursor-deeplink/mcp/install?name=truesight&config=eyJ1cmwiOiJodHRwczovL2FwaS50cnVlc2lnaHQuZ29vZGV5ZWxhYnMuY29tL21jcC8iLCJoZWFkZXJzIjp7IkF1dGhvcml6YXRpb24iOiJCZWFyZXIgWU9VUl9BUElfS0VZX0hFUkUifX0=)

Click the button, then replace `YOUR_API_KEY_HERE` with your platform API key in Cursor's MCP settings (`~/.cursor/mcp.json`).

**Claude Code:**

```bash
claude mcp add --transport http truesight \
  https://api.truesight.goodeyelabs.com/mcp/ \
  --header "Authorization: Bearer YOUR_API_KEY_HERE"
```

**Claude Desktop:**

Claude Desktop requires [Node.js](https://nodejs.org/) and the [`mcp-remote`](https://www.npmjs.com/package/mcp-remote) bridge (auto-installed via `npx`).

1. Open Claude
2. Go to **Settings** (gear icon) > **Developer** > **Edit Config**
3. Add the Truesight server to the `mcpServers` object in your `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "truesight": {
      "command": "npx",
      "args": [
        "mcp-remote",
        "https://api.truesight.goodeyelabs.com/mcp/",
        "--header",
        "Authorization:${TRUESIGHT_KEY}"
      ],
      "env": {
        "TRUESIGHT_KEY": "Bearer YOUR_API_KEY_HERE"
      }
    }
  }
}
```

4. Replace `YOUR_API_KEY_HERE` with your actual platform API key
5. Save the file and restart Claude

**VS Code (GitHub Copilot):**

[![Install in VS Code](https://img.shields.io/badge/VS_Code-Install_Server-0098FF?style=flat-square&logo=visualstudiocode)](vscode:mcp/install?%7B%22name%22%3A%20%22truesight%22%2C%20%22type%22%3A%20%22http%22%2C%20%22url%22%3A%20%22https%3A//api.truesight.goodeyelabs.com/mcp/%22%2C%20%22headers%22%3A%20%7B%22Authorization%22%3A%20%22Bearer%20YOUR_API_KEY_HERE%22%7D%7D) [![Install in VS Code Insiders](https://img.shields.io/badge/VS_Code_Insiders-Install_Server-24bfa5?style=flat-square&logo=visualstudiocode)](vscode-insiders:mcp/install?%7B%22name%22%3A%20%22truesight%22%2C%20%22type%22%3A%20%22http%22%2C%20%22url%22%3A%20%22https%3A//api.truesight.goodeyelabs.com/mcp/%22%2C%20%22headers%22%3A%20%7B%22Authorization%22%3A%20%22Bearer%20YOUR_API_KEY_HERE%22%7D%7D)

Click a button, then replace `YOUR_API_KEY_HERE` with your platform API key.

Or add manually to `.vscode/settings.json`:

```json
{
  "mcp": {
    "servers": {
      "truesight": {
        "url": "https://api.truesight.goodeyelabs.com/mcp/",
        "headers": {
          "Authorization": "Bearer YOUR_API_KEY_HERE"
        }
      }
    }
  }
}
```

**Windsurf:**

```json
{
  "mcpServers": {
    "truesight": {
      "serverUrl": "https://api.truesight.goodeyelabs.com/mcp/",
      "headers": {
        "Authorization": "Bearer YOUR_API_KEY_HERE"
      },
      "disabled": false
    }
  }
}
```

Replace `YOUR_API_KEY_HERE` with your actual platform API key and restart your client.
