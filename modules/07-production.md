# Production, Monitoring, and Business Value

## Plain-language overview

A demonstration proves an idea can work once. A production system must work repeatedly for real users, make failures visible, protect data, and provide enough value to justify its cost.

## Mental model

Treat an AI product as an operating service with three dashboards:

1. **Quality:** Is the system giving grounded, useful answers and choosing the right tools?
2. **Reliability:** Is it available, fast enough, and recovering from failures?
3. **Value:** Is it reducing effort, improving consistency, or supporting better decisions?

## What to version

- model and configuration;
- system instructions and prompt templates;
- tool definitions and schemas;
- retrieval settings and source collection;
- evaluation dataset and scoring logic;
- application code and deployment configuration.

Without versions, it is difficult to explain why behavior changed or to restore the last known good system.

## What to trace

A trace should make the journey of one request visible: input, retrieved sources, model calls, tool arguments and results, approvals, errors, response, timing, and cost. Protect sensitive data when logging.

## Operational measures

| Area | Useful measures |
|---|---|
| Quality | grounded-answer rate, citation accuracy, tool success, user rating |
| Speed | end-to-end latency and time spent in each step |
| Reliability | availability, error rate, retry rate, fallback rate |
| Cost | tokens, model cost, tool cost, cost per completed task |
| Data | freshness, missing records, schema failures, retrieval coverage |
| Adoption | active users, repeat use, completion rate, abandoned tasks |
| Value | time to diagnosis, analyst effort, decision turnaround, prevented errors |

## Analytics example

For an operational metrics agent, compare the old workflow with the AI-assisted workflow. The old process may require opening several Tableau workbooks, applying filters, comparing regions, and interpreting drivers. The new process can combine the metrics in one view and provide an evidence-backed explanation. Measure time to a correct diagnosis on a representative set of cases rather than guessing at savings.

## Release checklist

- A representative evaluation set meets defined thresholds.
- Access follows the user's role and data permissions.
- Sensitive tool actions require approval.
- Failures return a safe and understandable message.
- Traces and alerts expose quality and operational problems.
- Owners, rollback steps, and incident procedures are documented.
- Users understand limitations and how to report a problem.

## Common mistakes

- Monitoring uptime while ignoring answer quality.
- Changing prompts or models without rerunning evaluations.
- Logging sensitive content without retention and access controls.
- Launching without an owner for failed runs and user feedback.
- Reporting activity metrics without measuring a real business outcome.

## Review questions

1. Which parts of an AI application should be versioned together?
2. How would you detect a stale knowledge base?
3. What is the difference between a system health measure and a business-value measure?
4. What would make you roll back a new prompt or model?

## References

- [OpenAI Agents SDK tracing](https://openai.github.io/openai-agents-python/tracing/)
- [OpenAI evaluation best practices](https://platform.openai.com/docs/guides/evaluation-best-practices)
- [LangSmith observability concepts](https://docs.langchain.com/langsmith/observability-concepts)
- [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework)
