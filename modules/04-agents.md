# Module 4: Agents and agentic workflows

## Plain-language overview

An agent is a model working in a loop with instructions, tools, and state. It chooses the next step based on what it learns. A workflow follows steps chosen in advance. Reliable applications often use fixed workflows around small, bounded agent decisions.

## Goal

Build workflows that use tools and state reliably, with autonomy limited to what the task requires.

## Start simple

1. One agent with one clear responsibility
2. A small, well-described tool set
3. Structured results
4. Explicit stopping conditions
5. Human review for consequential actions

## Build progression

### Single analytics agent

The agent retrieves metric definitions, calls a diagnostic tool, and produces a traceable summary.

### Routed specialists

- Retrieval specialist: finds policies and definitions
- Metrics specialist: calculates comparisons and drivers
- Executive-writing specialist: converts findings into an audience-appropriate report

### Human review

Pause before sending reports, changing data, or triggering external actions. Display the proposed action, supporting evidence, and tool arguments.

## Engineering concerns

- Tool-selection accuracy
- State and context management
- Timeouts, retries, and idempotency
- Partial-result handling
- Trace inspection
- Cost and latency budgets
- Approval and escalation paths
- Clear distinction between evidence and inference

## Done when

- The agent completes a representative task set.
- Expected tools and arguments are tested.
- Failures recover safely or request help.
- Traces make decisions inspectable.
- The user remains in control of consequential actions.

## Common mistakes

- Adding multiple agents before one agent is reliable.
- Allowing an agent to continue without a stopping condition or budget.
- Giving every agent every tool.
- Confusing a persuasive explanation with a verified result.

## Review questions

1. When is a fixed workflow better than an agent?
2. What evidence would justify splitting one agent into specialists?
3. Which actions in an analytics agent should require approval?
4. What should a trace reveal after a failed run?

## References

- [OpenAI Agents guide](https://developers.openai.com/api/docs/guides/agents)
- [OpenAI Agents SDK](https://developers.openai.com/api/docs/guides/agents/sdk)
- [OpenAI practical guide to building agents](https://cdn.openai.com/business-guides-and-resources/a-practical-guide-to-building-agents.pdf)
