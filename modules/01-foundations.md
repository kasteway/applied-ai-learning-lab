# Module 1: Applied AI foundations

## Plain-language overview

A language model generates a response from instructions and context. It is powerful at interpreting and producing language, but it does not automatically have current company data, guaranteed facts, permissions, or the ability to act. The application around it supplies those capabilities.

## Mental model

Treat the model like a fast analyst with a temporary workspace. It can interpret what is placed in that workspace, but it needs clear instructions, relevant evidence, defined tools, and checks on its result.

## Goal

Build reliable model interactions before adding retrieval or agents.

## Learn

- Model inputs, outputs, tokens, and context limits
- System instructions and context engineering
- Structured outputs and JSON Schema
- Function calling and tool loops
- API authentication, timeouts, retries, and rate limits
- State, streaming, logging, latency, and cost

## Prompt and context engineering

A prompt states the task. Context engineering decides everything available to the model while it performs that task: system instructions, user input, conversation history, retrieved evidence, tool results, and state.

A useful business prompt defines the audience, trusted source, calculation rules, output structure, and behavior when evidence is missing. Extra context should earn its place; irrelevant text increases cost and can reduce accuracy.

## Build

Create a KPI diagnostic service that accepts a structured scorecard and returns:

```json
{
  "summary": "On-time performance declined 2.3 points.",
  "primary_driver": "West region volume increase",
  "evidence": ["volume +18%", "capacity +4%"],
  "confidence": "medium",
  "follow_up": ["Review Tuesday staffing variance"]
}
```

Validate the output schema and add tests for missing metrics, malformed data, and unsupported questions.

## Done when

- Output is machine-readable and validated.
- Errors are handled without exposing secrets or stack traces.
- Logs record latency and failures.
- The README explains limitations and business use.

## Worked example: from scorecard to explanation

Assume the input contains ten metrics split by global and regional performance. Before asking a model to explain anything, define the data contract:

```json
{
  "period": "2026-09",
  "metric": "service_level",
  "global": {"actual": 84.2, "prior": 87.3, "target": 88.0},
  "regions": [
    {"name": "West", "actual": 78.1, "prior": 84.3},
    {"name": "East", "actual": 89.0, "prior": 88.6}
  ]
}
```

The application should calculate exact changes in code. The model should interpret those validated calculations, identify patterns, and communicate them. This separation avoids asking a probabilistic model to perform work ordinary code can do exactly.

The response contract might require `summary`, `observed_drivers`, `unsupported_hypotheses`, `confidence`, and `missing_data`. This forces the system to distinguish what the data shows from what still needs investigation.

## Design decisions to practice

| Decision | Better default | Reason |
|---|---|---|
| Calculation | Deterministic code | Exact, testable, and repeatable |
| Explanation | Language model | Adapts complex findings to the audience |
| Output shape | Validated schema | Easier to test, display, and reuse |
| Missing evidence | Explicit abstention | Reduces invented explanations |
| Secrets | Environment or secret manager | Keeps credentials out of code and logs |

## Practice exercise

Create three scorecards: normal performance, a clear regional decline, and missing regional data. Write the input and output schemas, then describe how the system should behave in each case. Test whether another person could determine which parts are calculated facts and which parts are model-generated explanation.

## Review questions

1. What is inside a model's context window?
2. Why can more context sometimes make a result worse?
3. Why does low temperature improve repeatability without guaranteeing correctness?
4. Which responsibilities belong to the application rather than the model?

## References

- [OpenAI prompt engineering](https://platform.openai.com/docs/guides/prompt-engineering)
- [OpenAI text generation concepts](https://platform.openai.com/docs/guides/text-generation)
- [OpenAI production best practices](https://platform.openai.com/docs/guides/production-best-practices)
