# Module 5: Evaluation and observability

## Goal

Replace “the demo looks good” with repeatable evidence that the system works.

## Evaluation layers

| Layer | Example measure |
|---|---|
| Data | Freshness, completeness, valid values |
| Retrieval | Recall, relevance, citation coverage |
| Tools | Correct tool, valid arguments, successful result |
| Response | Factuality, groundedness, completeness, clarity |
| Workflow | Task completion, approval compliance, recovery |
| Operations | Latency, cost, error rate, availability |

## Build an evaluation dataset

Include:

- Common questions
- Ambiguous questions
- Missing-information cases
- Incorrect assumptions
- Access-restricted requests
- Tool failures
- Prompt-injection attempts
- Known historical regressions

## Use multiple evaluator types

- Deterministic code checks for schemas, citations, tool calls, and calculations
- Human review for usefulness and business accuracy
- Model-based graders with explicit rubrics for semantic qualities

## Release gate example

- 95% valid structured outputs
- 90% correct tool selection
- 90% citation support
- 0 unauthorized actions
- Defined latency and cost thresholds
- No regression on critical scenarios

