# Platform API Keys

Platform API keys let you access the Truesight REST API programmatically. Use them to integrate Truesight into scripts, CI/CD pipelines, AI agents, or any workflow that needs to create datasets, run evaluations, or read results without going through the web interface.

## Overview

Platform API keys are different from the per-endpoint Live Evaluation keys (`ts_sk_`). While Live Evaluation keys are scoped to a single evaluation endpoint, platform API keys can access multiple Truesight resources based on the scopes you assign.

| Feature | Platform API Keys (`ts_pat_`) | Live Evaluation Keys (`ts_sk_`) |
|---------|-------------------------------|----------------------------------|
| **Purpose** | General-purpose API access | Single evaluation endpoint |
| **Scope** | Multiple resources via scopes | One live evaluation |
| **Auth identity** | Acts as the creating user | Anonymous (no user context) |
| **Created from** | [Settings](/app/settings) page | [Live Evaluations](/app/live-evaluations) page |

## Creating a key

1. Navigate to [Settings](/app/settings) in the sidebar
2. In the **Truesight API Keys** section, click **Create Key**
3. Enter a descriptive name (e.g., "CI Pipeline", "MCP Client", "Data Sync Script")
4. Select the scopes your key needs (see [Available scopes](#available-scopes) below)
5. Optionally set an expiration (30, 60, 90, or 365 days, or leave it with no expiration)
6. Click **Create**

After creation, the full key is displayed once. Copy it immediately and store it securely. You will not be able to see the full key again.

## Using the key

Include your platform API key in the `Authorization` header of your HTTP requests:

```
Authorization: Bearer ts_pat_your_key_here
```

### cURL

```bash
curl -X GET "https://api.truesight.goodeyelabs.com/api/datasets" \
  -H "Authorization: Bearer ts_pat_your_key_here"
```

### Python

```python
import requests

response = requests.get(
    "https://api.truesight.goodeyelabs.com/api/datasets",
    headers={
        "Authorization": "Bearer ts_pat_your_key_here",
    },
)

datasets = response.json()
print(datasets)
```

### JavaScript / TypeScript

```typescript
const response = await fetch(
  "https://api.truesight.goodeyelabs.com/api/datasets",
  {
    headers: {
      "Authorization": "Bearer ts_pat_your_key_here",
    },
  }
);

const datasets = await response.json();
console.log(datasets);
```

## Available scopes

When creating a key, you choose which scopes to assign. Each scope grants access to specific API endpoints. A key can only access endpoints covered by its assigned scopes.

| Scope | What it allows |
|-------|---------------|
| `datasets:read` | List datasets, get dataset details, get rows, export |
| `datasets:write` | Create, upload, update, delete datasets; manage rows, columns, column roles, and judgment configs |
| `evaluations:read` | List evaluations, get evaluation details, list variants |
| `evaluations:write` | Create, update, delete evaluations; create variants; generate evaluations |
| `live-evaluations:read` | List and view live evaluations |
| `live-evaluations:write` | Deploy, update, deactivate, activate live evaluations; regenerate keys |
| `live-evaluations:execute` | Run a live evaluation via `POST /api/eval/{public_id}` |
| `results:read` | List evaluation runs, get run details |
| `review:read` | List and view review queue items |
| `review:write` | Flag, judge, reject, and promote review items |
| `error-analysis:execute` | Suggest error notes, suggest error categories, consolidate categories |

**Tip:** Assign only the scopes your key needs. For a read-only integration, select only the `:read` scopes. For a CI pipeline that runs evaluations, `live-evaluations:execute` may be all you need.

## Running evaluations with a platform key

Platform API keys with the `live-evaluations:execute` scope can call the same evaluation endpoint as Live Evaluation keys. The difference is that platform keys authenticate as your user, so you need access to the live evaluation being called.

```bash
curl -X POST "https://api.truesight.goodeyelabs.com/api/eval/live_abc123" \
  -H "Authorization: Bearer ts_pat_your_key_here" \
  -H "Content-Type: application/json" \
  -d '{
    "inputs": {
      "user_query": "How do I return a jacket?",
      "bot_response": "You can return any item within 30 days."
    }
  }'
```

See the [API Integration](/docs/api-integration) guide for details on the evaluation request format, response structure, and error handling.

## Managing keys

### Viewing keys

Your active keys are listed in the **Truesight API Keys** section of [Settings](/app/settings). Each key shows its name, prefix (first 12 characters), assigned scopes, creation date, and when it was last used.

### Revoking keys

To revoke a key, click the delete icon next to it and confirm. Revoked keys stop working immediately and are removed from the list. Revocation cannot be undone. Create a new key if needed.

## Access control

Platform API keys act as the user who created them. When a request is made with your key, Truesight checks your access to the requested resource using the same rules as the web interface:

- You can access resources you own
- You can access resources shared with you (with appropriate permission level)
- You can access resources shared with your teams

If you lose access to a resource (e.g., a share is removed), your API key will also lose access.

## Security best practices

- **Store keys securely.** Treat platform API keys like passwords. Use environment variables or a secrets manager, and never commit them to version control.
- **Use minimal scopes.** Only assign the scopes your integration needs.
- **Set expiration dates.** For temporary integrations, set an expiration so keys don't linger.
- **Rotate keys periodically.** Create a new key and revoke the old one on a regular schedule.
- **Revoke compromised keys immediately.** If a key is exposed, revoke it from [Settings](/app/settings) right away.

## Error handling

| Status | Meaning |
|--------|---------|
| `401` | Missing, invalid, expired, or revoked API key |
| `403` | Key is valid but missing the required scope for this endpoint |
| `402` | Subscription inactive or insufficient credits |
| `404` | Resource not found or you don't have access |

Error responses include a `detail` field:

```json
{
  "detail": "Insufficient API key scopes. Required: ['datasets:read']"
}
```

## MCP integration

Platform API keys are also used to authenticate MCP (Model Context Protocol) connections. After creating a key, the key reveal dialog includes a pre-filled MCP configuration you can copy directly into Claude or Cursor. See the [MCP Integration](/docs/mcp-integration) guide for setup instructions.
