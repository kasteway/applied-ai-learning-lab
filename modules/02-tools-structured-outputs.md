# Tools, APIs, and Structured Outputs

## Plain-language overview

Models naturally produce text. Business applications often need dependable fields, calculations, or current information. Structured outputs make responses machine-readable, while tools let the model request work from ordinary software.

## Mental model

Think of the model as an analyst filling out a controlled request form:

1. The application describes an available tool and its required inputs.
2. The model decides whether the tool is needed.
3. The model proposes a tool name and arguments.
4. The application validates permissions and arguments.
5. Normal code runs the tool.
6. The result returns to the model for interpretation.

The model does not secretly execute the function. The surrounding application remains in control.

## Three concepts to separate

| Concept | Purpose | Example |
|---|---|---|
| Structured output | Make the model's response follow a schema | Return driver, impact, evidence, and confidence as JSON |
| Tool calling | Let the model request an approved function | Call `compare_periods(metric, region)` |
| API | Let software systems exchange requests and responses | Retrieve a current scorecard from a governed service |

## Analytics example

A user asks, “Why did global service level fall?” The agent calls a comparison tool, receives regional variances, and returns:

```json
{
  "metric": "global_service_level",
  "change": -3.1,
  "primary_driver": "West region",
  "driver_change": -6.2,
  "evidence_status": "supported"
}
```

The schema makes it easier to display the result in an HTML dashboard, store it, test it, or pass it to another step.

## Good tool design

- Use a clear verb-based name.
- Describe when the tool should and should not be used.
- Require only necessary arguments.
- Constrain values with types, enums, ranges, and formats.
- Return a predictable success or error shape.
- Keep permissions narrow and default to read-only access.
- Log the request, result, user, and timing.

## Common mistakes

- Using free-form output when downstream software expects exact fields.
- Trusting model-generated arguments without validation.
- Giving one tool broad access to unrelated systems.
- Returning raw, oversized data when a focused result would be safer and clearer.
- Retrying an action without considering whether the first attempt partly succeeded.

## Worked example: a driver-analysis tool set

Avoid one broad tool such as `analyze_everything`. Give each tool a clear contract:

```text
get_scorecard(period, scope)
compare_periods(metric, current_period, prior_period, scope)
find_metric_drivers(metric, period, dimension, limit)
get_metric_definition(metric)
```

The first tool retrieves governed values. The second calculates change. The third ranks contributions across an approved dimension. The fourth retrieves the definition, owner, unit, and freshness rule. A model can combine them, but each individual result remains understandable and testable.

### Example failure response

```json
{
  "status": "error",
  "error_code": "PERIOD_NOT_AVAILABLE",
  "message": "October 2026 data has not completed validation.",
  "available_through": "2026-09-30",
  "retryable": false
}
```

Useful errors help the model respond safely instead of guessing or repeatedly calling the same tool.

## Practice exercise

Design a schema for `find_metric_drivers`. Specify required inputs, allowed dimensions, maximum result count, user permissions, success output, and three error cases. Then write two examples: one request that should run automatically and one that should require approval.

## Review questions

1. What does the model control during a tool call, and what does the application control?
2. Why is a schema useful even when the final result will be shown as prose?
3. What validation would `get_scorecard(metric, period, region)` need?
4. Which tool actions should require human approval?

## References

- [OpenAI tools guide](https://developers.openai.com/api/docs/guides/tools)
- [OpenAI function calling](https://developers.openai.com/api/docs/guides/function-calling)
- [OpenAI structured outputs](https://developers.openai.com/api/docs/guides/structured-outputs)
- [JSON Schema documentation](https://json-schema.org/learn/getting-started-step-by-step)
