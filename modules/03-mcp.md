# Module 3: Model Context Protocol

**Worked code:** See the [OpsAtlas MCP walkthrough](../STUDY_GUIDE.md#from-opsatlas-the-entire-mcp-idea-in-a-few-lines) for the actual server function, a line-by-line explanation, and what its client can request.

## Plain-language overview

MCP gives AI applications a consistent way to discover and use external tools, readable resources, and reusable prompts. It is similar to using a standard connector shape across many systems.

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

## Worked example: an operational metrics MCP server

The server can expose a small, read-only capability set. OpsAtlas currently implements two tools; the resources and prompt below are design examples, not published OpsAtlas features:

| MCP capability | Example | Why it fits |
|---|---|---|
| Tool | `compare_periods` | Performs a parameterized calculation |
| Tool | `find_metric_drivers` | Runs a governed diagnostic query |
| Resource | `metrics://definitions/service-level` | Readable reference information |
| Resource | `metrics://freshness/current` | Current validation and refresh status |
| Prompt | `regional-performance-review` | Reusable user-selected analysis template |

The tool should return source version, calculation timestamp, units, filters, and data-freshness status along with the result. This lets the host explain where the answer came from.

## Design exercise: resource or tool?

Use a **resource** when the client mainly needs to read identified context. Use a **tool** when inputs trigger a calculation, query, or action. A metric definition is naturally a resource; comparing two periods is naturally a tool.

## Practice exercise

Draft the interface for a read-only MCP server without writing code. List its tools, resources, and prompts; define who may use each; identify untrusted inputs; and specify which fields must appear in its audit log. Then explain how the same server could be used by more than one AI host.

## Common mistakes

- Assuming that a standard connection is automatically a safe connection.
- Giving a server more data or permissions than its tasks require.
- Using vague tool names and descriptions that make selection unreliable.
- Letting untrusted server content override application rules.

## Review questions

1. What are the responsibilities of the host, client, and server?
2. When should something be exposed as a resource rather than a tool?
3. Which controls still need to exist outside MCP?
4. Why begin with read-only capabilities?

## References

- [Model Context Protocol documentation](https://modelcontextprotocol.io/)
- [OpenAI MCP and connectors guide](https://developers.openai.com/api/docs/guides/tools-connectors-mcp)
