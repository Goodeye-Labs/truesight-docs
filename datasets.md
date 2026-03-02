# Datasets

Datasets are the foundation of everything in Truesight. They contain the AI inputs and outputs you want to evaluate, and they're used across the entire platform.

## What is a dataset?

A dataset is a collection of examples, where each example represents one interaction with your AI system. At minimum, each example has an **input** (what was sent to the AI) and an **output** (what the AI produced).

You can also include additional columns like:

- **Trace metadata**: Tool calls, chain-of-thought reasoning, retrieval context, or other intermediate steps from your AI pipeline
- **Expert reviews**: Human judgments, corrections, or quality annotations for each example
- **Other metadata**: Timestamps, user IDs, model versions, session IDs, or any other information useful for filtering and analysis

## Supported formats

Truesight accepts three file formats:

### CSV

Standard comma-separated values with a header row. Best for tabular data exported from spreadsheets or databases.

| user_query | bot_response |
|------------|--------------|
| How do I return a jacket? | You can return any item within 30 days. Visit your Orders page and select Return Item. |
| Do you ship to Canada? | Yes, we ship to Canada. Standard delivery takes 5-7 business days. |
| My order arrived damaged | Please send a photo of the damage and we'll ship a replacement right away. |

### JSON

An array of objects where each object is one example. Best for structured data with nested fields.

```json
[
  {
    "user_query": "How do I return a jacket?",
    "bot_response": "You can return any item within 30 days. Visit your Orders page and select Return Item."
  },
  {
    "user_query": "Do you ship to Canada?",
    "bot_response": "Yes, we ship to Canada. Standard delivery takes 5-7 business days."
  }
]
```

### JSONL

One JSON object per line (newline-delimited JSON). Best for large datasets or streaming data exports. Each line is a complete JSON object:

```json
{
  "user_query": "How do I return a jacket?",
  "bot_response": "You can return any item within 30 days."
}
```

### OpenAI Chat Completions format

Truesight supports the [OpenAI Chat Completions](https://platform.openai.com/docs/api-reference/chat/create) conversation format, which has become the standard way most AI tools and platforms represent chat-style interactions. If you use a logging, tracing, or observability tool that exports conversations in this format, you can upload them directly to Truesight as a JSON or JSONL file.

```json
{
  "messages": [
    { "role": "system", "content": "You are a helpful retail support agent." },
    { "role": "user", "content": "How do I return a jacket?" },
    { "role": "assistant", "content": "You can return any item within 30 days. Visit your Orders page and select Return Item." }
  ]
}
```

When a column contains data in this format, Truesight automatically detects it and renders the conversation with role-based styling.

## Selecting content to evaluate

All columns are visible by default when you review data. You can toggle columns on and off directly from the annotation or review interface using the column visibility controls.

Optionally, you can designate specific columns from the dataset detail page:

- **Text Content**: Select one or more columns that contain the text to evaluate. If you select multiple columns, they'll be combined together with labels so the evaluator can see all of them. For example, you might select both a "user_query" column and a "bot_response" column.
- **Image URL**: Optionally select a column that contains image URLs, for evaluations that include image content.

### Automatic column type detection

When you upload a dataset, Truesight automatically detects which columns contain image URLs. A column is classified as an image URL column when more than half of the non-empty cells in a sample of the first 50 rows contain only a URL pointing to an image file (e.g., `.png`, `.jpg`, `.webp`). Cells with additional text alongside the URL are not counted. The occasional malformed or missing value won't prevent a column from being detected.

Detected image URL columns are pre-selected for you in the column roles step after upload, and all remaining columns are pre-selected as text content. You can always adjust these selections before confirming.

If no content columns are explicitly set, all text columns are shown by default. You can change these selections at any time from the dataset detail page by clicking the more actions menu and choosing **Edit Details**.

## Managing datasets

### Viewing and exploring

From the [Datasets](/app/datasets) page, you can see all your datasets with their row counts, column information, and creation dates. Select a dataset to view its contents, edit its settings, or use it in other features.

### Editing

You can update which columns are used for evaluation at any time from the dataset detail page via the more actions menu. Changing these selections will affect future evaluations but won't retroactively change existing results.

### Promoting from Review Queue

When you review evaluation results and flag corrections, those corrections can be promoted back into your dataset. This creates a feedback loop where your dataset improves over time based on evaluation insights. See [Results & Review](/docs/results-review) for details on the Review Queue workflow.

## Best practices

- **Start small**: 20 to 50 examples is enough to build and test initial evaluations. You can always add more later.
- **Include variety**: Make sure your dataset covers different types of inputs, edge cases, and quality levels.
- **Curate intentionally**: A small, well-curated dataset is more valuable than a large, noisy one.
- **Version your data**: Upload new datasets rather than overwriting existing ones, so you can track how your evaluations perform over time.
- **Use meaningful column names**: Clear column names make it easier to assign roles and understand results.
