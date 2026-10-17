# Module 4: Agents and agentic workflows

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

