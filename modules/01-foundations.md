# Module 1: Applied AI foundations

## Goal

Build reliable model interactions before adding retrieval or agents.

## Learn

- Model inputs, outputs, tokens, and context limits
- System instructions and context engineering
- Structured outputs and JSON Schema
- Function calling and tool loops
- API authentication, timeouts, retries, and rate limits
- State, streaming, logging, latency, and cost

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

