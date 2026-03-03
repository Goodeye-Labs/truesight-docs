# Truesight Documentation

Welcome to the Truesight documentation. Truesight is an AI evaluation platform that helps you build expert-grounded evaluations for your AI applications, no coding required.

## What is Truesight?

Truesight lets you create custom evaluation criteria based on your domain expertise, then deploy those evaluations as live API endpoints. Instead of relying on generic metrics, you define what "good" and "bad" look like for your specific use case.

## Who is this for?

- **Domain experts** who want to evaluate AI outputs based on their professional knowledge
- **Product teams** building AI-powered features who need reliable quality signals
- **Developers** integrating evaluation into their CI/CD pipelines or production systems

## Core workflow

Truesight follows a structured workflow to turn your expertise into automated evaluations:

Fast path for first-time users:

1. **Start from a template**: Pick a pre-built template from the Dashboard to provision a labeled starter dataset and deploy quickly
2. **Customize and iterate**: Review criteria, adjust labels, and redeploy as needed

Standard path:

1. **Upload your data**: Bring in datasets of AI inputs and outputs you want to evaluate
2. **Review and label**: Use [Error Analysis](/docs/error-analysis) to identify patterns and failure modes in your data
3. **Build evaluations**: Turn your findings into structured evaluation criteria using [Guided Setup](/docs/evaluations)
4. **Test and refine**: Run evaluations on your data and iterate on your criteria
5. **Deploy**: Publish evaluations as live API endpoints for production use

## Use Cases

Once you've built and deployed evaluations, you can integrate them across your AI development lifecycle:

- **Development**: Quality bars for your developers to measure and tune AI features against as they build
- **Pre-Deployment**: Regression detection when you change prompts or switch models. Catch issues before they reach production.
- **Production**: Continuous monitoring for drift before your users notice problems
- **Guardrails**: Block known error modes before they reach users. Integrate evaluations as a safety layer in your AI pipeline.

See the [API Integration](/docs/api-integration) guide to learn how to deploy evaluations for these use cases.

## Key concepts

### Datasets

Datasets contain the AI inputs and outputs you want to evaluate. You upload them as CSV, JSON, or JSONL files. All columns are visible by default, and you can toggle which columns to show while reviewing data. See the [Datasets](/docs/datasets) guide for supported formats and best practices.

### Error Analysis

Error Analysis is a two-phase process for discovering evaluation criteria from your data:

- **Open Coding**: Review individual examples and note patterns you observe
- **Axial Coding**: Group related observations into categories that become evaluation criteria

See the [Error Analysis](/docs/error-analysis) guide for a detailed walkthrough.

### Evaluations

Evaluations are the core building blocks. Each evaluation defines a set of criteria and uses AI judges to assess whether outputs meet those criteria. Assessment methods include binary pass/fail checks, categorical classifications, and graded scoring, giving you flexibility in how criteria are scored. Truesight supports multiple model providers, so you can choose the best judge for each criterion. The [Guided Setup](/docs/evaluations) wizard walks you through creating evaluations step by step.

### Live Evaluations

Once you're satisfied with an evaluation, you can deploy it as a [live API endpoint](/docs/api-integration). This gives you a URL and API key that you can call from your application to evaluate outputs in real time.

### Results and Review Queue

Results show evaluation outcomes across your datasets. The Review Queue lets you flag and review individual results, promoting corrections back into your datasets for continuous improvement. See [Results & Review](/docs/results-review) for details.

### Credits and API Keys

Evaluations consume credits from your balance when using Truesight's Managed API keys. You can bring your own provider API keys (at the user or organization level) which take priority over Managed API keys. When configured, your custom keys are used and credits are not consumed. Credits can be purchased directly from within the platform. See [Teams & Organizations](/docs/teams-organizations) for details on managing API keys and credits.

## Next steps

- **New to Truesight?** Start with the [Getting Started](/docs/getting-started) guide
- **Exploring features?** Browse the feature guides in the sidebar
- **Ready to integrate?** Check the [API Integration](/docs/api-integration) guide
- **Need programmatic access?** Set up [Platform API Keys](/docs/platform-api-keys)
- **Using AI assistants?** Connect via [MCP Integration](/docs/mcp-integration)
