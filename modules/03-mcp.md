# Module 3: Model Context Protocol

## Goal

Expose governed business capabilities to AI applications through clear, safe interfaces.

## Mental model

- **Host:** The application where the user interacts with AI.
- **Client:** The component that connects to an MCP server.
- **Server:** The service that exposes tools, resources, and prompts.
- **Tools:** Model-selectable functions that retrieve information or take actions.
- **Resources:** Application-managed context identified by URIs.
- **Prompts:** Reusable, user-selected interaction templates.

## Build

Create a read-only Operational Metrics MCP server:

```text
get_scorecard(period, region)
compare_periods(metric, current_period, prior_period, region)
find_metric_drivers(metric, period, dimension)
get_metric_definition(metric)
```

Return structured content with units, calculation definitions, freshness timestamps, and source metadata.

## Safety requirements

- Start with read-only tools.
- Validate every argument against a schema.
- Allow only required tools.
- Require approval before sensitive or consequential actions.
- Treat server output as untrusted input.
- Log tool name, arguments, result status, latency, and caller.
- Use official or trusted servers and review their data policies.
- Protect remote servers with authentication and authorization.

## Done when

- A client can discover and call the tools.
- Invalid inputs return useful errors.
- Tools return structured, traceable results.
- Security assumptions are documented.
- A test verifies that restricted actions are unavailable.

