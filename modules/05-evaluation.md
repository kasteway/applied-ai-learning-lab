# Module 5: Evaluation and observability

## Plain-language overview

Evaluation is a repeatable way to compare system behavior with an expected result or rubric. Observability records what happened during real runs so failures can be found and explained.

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

## Worked example: evaluation record

```text
Case: global_service_decline_07
Question: Why did global service level decline?
Required evidence: West region contribution; staffing variance
Expected tools: compare_periods, find_metric_drivers
Forbidden behavior: claim causation without supporting data
Scoring: calculation 2, evidence 2, tool use 2, clarity 1, uncertainty 1
Critical failure: unauthorized region data or invented driver
```

Keep the input data and expected evidence fixed when comparing models or prompts. Otherwise you cannot tell whether the configuration change caused the result.

## Failure analysis

Label each failure by its earliest cause: bad source data, missing retrieval, wrong tool, invalid calculation, unsupported generation, permission failure, or unclear presentation. Fixing the earliest broken stage usually produces a more durable improvement than rewriting the final prompt.

## Practice exercise

Build a 15-case evaluation set: five common cases, three ambiguous questions, two missing-data cases, two access-restricted requests, two tool failures, and one prompt-injection attempt. Define the expected behavior and one critical-failure condition for every case.

## Common mistakes

- Testing only easy examples used during development.
- Using one overall score that hides the failing stage.
- Treating a model-based grader as unquestionable.
- Improving average quality while allowing a critical safety regression.

## Review questions

1. Which checks should use exact code and which need human judgment?
2. What belongs in a representative evaluation dataset?
3. How would you isolate a retrieval failure from a generation failure?
4. Why should critical scenarios have their own release gate?

## References

- [OpenAI evaluation best practices](https://platform.openai.com/docs/guides/evaluation-best-practices)
- [OpenAI evals guide](https://platform.openai.com/docs/guides/evals)
- [LangSmith evaluation concepts](https://docs.langchain.com/langsmith/evaluation-types)
