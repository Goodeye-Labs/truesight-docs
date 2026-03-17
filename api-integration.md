# API Integration

Truesight provides a public API endpoint for running evaluations programmatically. Once you've built and tested an evaluation in the platform, you can deploy it as a live endpoint and call it from your application.

## Overview

The API lets you send AI inputs and outputs to Truesight and receive evaluation results in real time. This enables four key use cases:

- **Development**: Quality bars for your developers to measure and tune AI features against as they build
- **Pre-Deployment**: Regression detection when you change prompts or switch models. Catch issues before they reach production.
- **Production**: Continuous monitoring for drift before your users notice problems
- **Guardrails**: Block known error modes before they reach users. Integrate evaluations as a safety layer in your AI pipeline.

## Authentication

Truesight supports two types of API keys:

**Live Evaluation keys** (`ts_sk_`) are per-endpoint keys generated when you activate a Live Evaluation. Each Live Evaluation has its own unique key, scoped to that single endpoint. You can view them from the [Live Evaluations](/app/live-evaluations) page.

```
Authorization: Bearer ts_sk_your_api_key_here
```

**Platform API keys** (`ts_pat_`) provide broader access to the Truesight API based on assigned scopes. Create them from the [Settings](/app/settings) page. Platform keys authenticate as your user and can access any endpoint their scopes allow, including the evaluation endpoint. See the [Platform API Keys](/docs/platform-api-keys) guide for details.

```
Authorization: Bearer ts_pat_your_key_here
```

Both key types use the same `Authorization: Bearer` header format. **Keep your API keys secure.** Treat them like passwords. Don't commit them to version control or expose them in client-side code.

## Endpoint

```
POST /api/eval/{public_id}
```

The `public_id` is the unique identifier for your Live Evaluation, shown on the [Live Evaluations](/app/live-evaluations) page.

### Request

Send a JSON body with your input data:

```json
{
  "inputs": {
    "user_query": "How do I return a jacket?",
    "bot_response": "You can return any item within 30 days. Visit your Orders page and select Return Item."
  },
  "include_owner": true
}
```

The keys in `inputs` must match the input column names configured in your evaluation's dataset. Column name matching is case-insensitive. For image-only evaluations (no text content columns configured), `inputs` can be an empty object -- just provide `media_url` instead.

#### Optional fields

| Field | Type | Description |
|-------|------|-------------|
| `media_url` | string | URL to an image for evaluations that include image content. Required for image-only evaluations. |
| `include_owner` | boolean | When `true`, includes the Live Evaluation owner's name (or email) in the response under the `owner` field. Defaults to `false`. |

### Response

A successful evaluation returns:

```json
{
  "run_id": "run_a1b2c3d4e5f67890",
  "results": {
    "factual_accuracy": {
      "final_output": "pass",
      "reasoning": "The response correctly states the 30-day return policy and accurately describes the return process through the Orders page."
    },
    "response_completeness": {
      "final_output": "fail",
      "reasoning": "The response mentions the return window but does not specify whether the customer needs to pay for return shipping or if a prepaid label is provided."
    }
  },
  "owner": "Kevin Mitchell"
}
```

- `run_id` is a unique identifier for this evaluation run
- `results` is an object containing the judgment and reasoning for each evaluation criterion
- `owner` is the name (or email) of the Live Evaluation's owner, only present when `include_owner` is `true` in the request

## Error handling

| Status | Meaning |
|--------|---------|
| `200` | Evaluation completed successfully |
| `401` | Missing or invalid API key |
| `402` | Subscription inactive or insufficient credits (see [Credits and billing](#credits-and-billing) below) |
| `404` | Live Evaluation not found or inactive |
| `422` | Invalid request (missing required inputs, validation errors) |
| `429` | Rate limit exceeded |
| `500` | Internal error during evaluation |

Error responses include a `detail` field with a human-readable message:

```json
{
  "detail": "Missing required input columns: user_query, bot_response"
}
```

## Rate limits

Each Live Evaluation endpoint is rate-limited to **60 requests per minute** by default. If you exceed the limit, you'll receive a `429` response. Wait and retry after a brief delay.

## Credits and billing

Each API call consumes credits from your organization's balance when using Truesight's Managed API keys. If your balance reaches zero, API calls will return a `402` error.

To add credits, visit the [Usage & Balance](/app/usage) page in the sidebar. You can purchase credits instantly using preset amounts ($25, $50, $100, $250) or enter a custom amount ($5 to $1,000). Payments are processed through Stripe.

**Using your own API keys (BYOK)**: Available on the **Enterprise** plan. When configured, your keys take priority and credits are not consumed. See [Teams & Organizations](/docs/teams-organizations) for details on managing credits and API keys.

## Examples

### cURL

```bash
curl -X POST "https://api.truesight.goodeyelabs.com/api/eval/live_abc123" \
  -H "Authorization: Bearer ts_sk_your_api_key_here" \
  -H "Content-Type: application/json" \
  -d '{
    "inputs": {
      "user_query": "How do I return a jacket?",
      "bot_response": "You can return any item within 30 days. Visit your Orders page and select Return Item."
    }
  }'
```

### Python

```python
import requests

response = requests.post(
    "https://api.truesight.goodeyelabs.com/api/eval/live_abc123",
    headers={
        "Authorization": "Bearer ts_sk_your_api_key_here",
    },
    json={
        "inputs": {
            "user_query": "How do I return a jacket?",
            "bot_response": "You can return any item within 30 days. Visit your Orders page and select Return Item.",
        }
    },
)

result = response.json()
print(result["results"])
```

### JavaScript / TypeScript

```typescript
const response = await fetch(
  "https://api.truesight.goodeyelabs.com/api/eval/live_abc123",
  {
    method: "POST",
    headers: {
      "Authorization": "Bearer ts_sk_your_api_key_here",
      "Content-Type": "application/json",
    },
    body: JSON.stringify({
      inputs: {
        user_query: "How do I return a jacket?",
        bot_response: "You can return any item within 30 days. Visit your Orders page and select Return Item.",
      },
    }),
  }
);

const result = await response.json();
console.log(result.results);
```

## Setting up a Live Evaluation

Before using the API, you need to activate a Live Evaluation in the platform:

1. Build and test an evaluation using [Guided Setup](/app/guided-setup) or the [Evaluations](/app/evaluations) page
2. Navigate to [Live Evaluations](/app/live-evaluations) in the sidebar
3. Select your evaluation and click **Activate** to deploy it
4. Copy the **API key** from the credentials dialog that appears after activation

The Live Evaluation captures a snapshot of your evaluation configuration at the time of activation. Changes to the original evaluation won't affect the live endpoint. To deploy updated criteria, activate a new Live Evaluation.

## Beyond the evaluation endpoint

The evaluation endpoint documented above is the most common API integration. If you need to programmatically manage datasets, evaluations, results, or review items, use a [Platform API Key](/docs/platform-api-keys) with the appropriate scopes.

For AI assistant integrations (Claude, ChatGPT, Cursor, etc.), Truesight also supports MCP (Model Context Protocol). See the [MCP Integration](/docs/mcp-integration) guide for setup instructions.

## Best practices

- **Test in the platform first**: Always validate your evaluation criteria on sample data before deploying to production.
- **Monitor results**: Evaluation runs are logged in the platform so you can [review outcomes](/docs/results-review) and spot issues.
- **Rotate API keys**: Regenerate keys periodically and after any suspected compromise.
- **Handle errors gracefully**: Implement retry logic with exponential backoff for transient failures (429, 500).
- **Cache when appropriate**: If you're evaluating the same input/output pair multiple times, consider caching results.
