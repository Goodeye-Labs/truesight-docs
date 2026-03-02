# Results & Review

After running evaluations, Truesight provides tools to analyze outcomes and feed insights back into your workflow. The [Results](/app/results) page shows evaluation outcomes, while the [Review Queue](/app/review-queue) lets you flag and correct individual assessments.

## Viewing results

Navigate to [Results](/app/results) in the sidebar to see evaluation outcomes across your datasets. Results display:

- **Per-example judgments**: The final assessment for each example in your dataset
- **Criteria breakdowns**: How each criterion was scored
- **Judge reasoning**: The rationale behind each judgment

For example, given this input:

```json
{
  "user_query": "How do I return a jacket?",
  "bot_response": "You can return any item within 30 days. Visit your Orders page and select Return Item."
}
```

The results might show:

| Criterion | Judgment | Reasoning |
|-----------|----------|-----------|
| Factual Accuracy | **Pass** | The response correctly states the 30-day return policy and accurately describes the return process through the Orders page. |
| Response Completeness | **Fail** | The response mentions the return window but does not specify whether the customer needs to pay for return shipping or if a prepaid label is provided. |

This level of detail helps you understand exactly why each judgment was made and where to focus improvements.

### Filtering and sorting

You can filter results by:

- **Live evaluation**: Filter to a specific live evaluation
- **Status**: Filter by outcome (Success or Failed)

This helps you focus on the examples that matter most. For instance, you can filter to see only failed evaluations. You can also export results for further analysis.

## Review Queue

The [Review Queue](/app/review-queue) is where you manually assess evaluation results that need human attention. It creates a feedback loop between automated evaluation and human expertise.

### How it works

1. **Flag** results from the Results page that you want to examine more closely
2. The Review Queue collects flagged items, grouped by evaluation run
3. Click **Review** to open a review session where you examine each judgment
4. For each item, you can **Approve** the evaluation's judgment or **Reject** it
5. On the final judgment, choose **Approve & Add to Dataset** to save approved results back to your dataset

### When to use the Review Queue

- **Disagreements**: When the evaluation's judgment doesn't match your expert judgment
- **Edge cases**: Examples that are genuinely ambiguous or unusual
- **Calibration**: Periodically reviewing a sample to ensure evaluation quality stays high
- **Training data**: Building a curated dataset of examples with verified labels

### Adding approved results to your dataset

When you approve results in the Review Queue, you can add them back into your dataset using the **Approve & Add to Dataset** action. This:

- Improves your [datasets](/docs/datasets) quality over time
- Provides better training examples for future [evaluations](/docs/evaluations)
- Creates a documented record of expert decisions

## Continuous improvement cycle

Results and Review form a continuous improvement loop:

1. **Run evaluations** on your data
2. **Review results** to identify where the evaluation succeeds and fails
3. **Flag edge cases** for the Review Queue
4. **Approve and add** reviewed results back to your dataset
5. **Refine evaluations** based on what you've learned
6. **Re-run** to measure improvement

This cycle ensures your evaluations stay aligned with your expert judgment as your AI system and data evolve.

## Best practices

- **Review regularly**: Don't let the Review Queue build up. Regular review keeps your evaluations calibrated.
- **Focus on disagreements**: The most valuable reviews are where you disagree with the evaluation.
- **Document your reasoning**: When overriding a judgment, note why. This helps when refining criteria later.
- **Track trends**: Monitor overall pass rates over time to detect drift in your AI system's quality.
