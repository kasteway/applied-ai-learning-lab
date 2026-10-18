# Applied AI Study Guide

This is the compact reference to read from beginning to end. Each section answers four questions: **What is it? Why does it matter? How does it work? What should I remember?**

## 1. The central idea

An AI application is a system built around a model. The model interprets language and generates a response, but the surrounding application determines what information it receives, which actions it can take, how its output is checked, and what gets recorded.

Think of the system as seven layers:

1. **Instructions** — the goal, role, rules, and output expectations.
2. **Context** — the information placed into the model's current working window.
3. **Retrieval** — a search step that finds relevant trusted information.
4. **Tools** — functions or services that retrieve live data or perform actions.
5. **State** — information remembered across the steps of a task.
6. **Evaluation** — tests that measure whether the result is good enough.
7. **Controls** — permissions, approvals, logging, and security boundaries.

**Remember:** a fluent answer is not proof of a correct or well-designed system.

## 2. Models, tokens, and context

A large language model predicts useful sequences of **tokens**, which are pieces of text. It does not query a database unless the application gives it a tool to do so. It does not automatically know a company's current metrics or private definitions.

The **context window** is the model's working memory for one interaction. It can contain instructions, conversation history, retrieved documents, tool results, and the user's request. More context is not always better: irrelevant material can distract the model, raise cost, and increase latency.

**Temperature** influences variation in generation. Lower values are usually preferred for extraction, classification, and repeatable business tasks. Higher values can be useful for brainstorming, but they do not add knowledge.

**Analytics example:** a model can explain a decline in service level only after it receives the relevant values, definitions, comparison period, and business constraints.

**Remember:** models reason over what they receive; applications control what they receive.

## 3. Prompt and context engineering

A prompt is the instruction sent to a model. Good prompts usually specify:

- the task and intended audience;
- the trusted information to use;
- definitions and business rules;
- the required output format;
- what to do when evidence is missing;
- examples when the task is ambiguous.

**Context engineering** is broader. It means designing the complete set of information available at each step: system instructions, user input, retrieved evidence, tool results, state, and prior messages.

A useful diagnostic prompt might say: analyze only the supplied scorecard, compare current versus prior period, rank the top three drivers, cite the supporting dimensions, and state when evidence is insufficient.

**Common mistake:** adding a long persona while leaving the business rules and evidence requirements vague.

**Remember:** tell the model what success looks like and give it the evidence needed to succeed.

## 4. Structured outputs and tools

Free-form text is easy for people to read but hard for software to depend on. A **structured output** follows a defined schema, such as JSON with required fields and data types.

```json
{
  "metric": "service_level",
  "direction": "down",
  "top_driver": "region_west",
  "confidence": "medium",
  "evidence": ["West declined 6.2 points"]
}
```

A **tool** is a function the model can request. The application receives the request, validates the arguments, runs the function, and returns the result to the model. A tool might fetch a scorecard, compare periods, calculate a variance, or search a policy library.

Tool descriptions and schemas matter because the model uses them to decide which tool to call and how to fill its arguments. The application must still enforce authorization, validation, timeouts, and error handling.

**Remember:** the model proposes a tool call; the application controls whether and how it runs.

## 5. Embeddings and vector search

An **embedding** represents the meaning of text as a list of numbers. Texts with similar meanings tend to have vectors that are close together. **Vector search** uses that distance to find relevant passages even when the wording differs.

Example: “Why did wait time rise?” may retrieve a document section titled “Drivers of handle-time variance” even though the exact words do not match.

Embeddings are useful for semantic search, clustering, recommendations, and retrieval. They do not verify that a source is true, current, or authorized. Metadata such as document owner, effective date, region, and access level remains essential.

**Remember:** embeddings help find meaning; governance determines which information may be trusted and used.

## 6. Retrieval-Augmented Generation (RAG)

RAG gives the model relevant information at the time of the request. A typical flow is:

1. collect and clean trusted documents;
2. split them into useful chunks;
3. attach metadata and create embeddings;
4. retrieve likely relevant chunks;
5. optionally rerank or filter them;
6. ask the model to answer from that evidence;
7. return citations and allow a “not found” result.

RAG is a strong choice when knowledge changes often, belongs to an organization, or needs source attribution. Fine-tuning is more useful for changing repeated behavior or style; it is generally not the first choice for injecting frequently changing facts.

Measure retrieval and generation separately. A weak answer may come from a missing source, poor chunking, irrelevant retrieval, or incorrect synthesis.

**Remember:** retrieval finds the evidence; generation explains it.

## 7. Model Context Protocol (MCP)

MCP is a standard way for AI applications to discover and use external capabilities. The three main roles are:

- **Host:** the AI application the user works in.
- **Client:** the connection inside the host that communicates using MCP.
- **Server:** the service exposing capabilities.

An MCP server can expose:

- **Tools** for actions or computations;
- **Resources** for readable data or documents;
- **Prompts** for reusable interaction templates.

For an operational analytics use case, an MCP server could safely expose `get_scorecard`, `compare_periods`, and `find_metric_drivers`. Clear schemas, read-only defaults, allowed-tool lists, authentication, logs, and approvals reduce risk.

MCP standardizes the connection; it does not automatically make the connected system safe or the returned data correct.

**Remember:** MCP is a common adapter between AI applications and governed capabilities.

## 8. Agents and workflows

An **agent** is a model operating inside a loop with instructions, tools, and state. It examines the task, chooses a next step, uses a tool when needed, observes the result, and continues until it reaches a stopping condition.

Use an agent when the next step depends on what the system discovers. Use a fixed workflow when the steps are already known. Many reliable systems combine both: deterministic stages around a limited agent decision.

A multi-agent design may assign narrow roles, such as retrieval specialist, metrics analyst, and executive-summary writer. More agents also mean more handoffs, latency, cost, and failure points. Start with one agent and split responsibilities only when the separation improves quality or control.

Human approval belongs before consequential, expensive, or irreversible actions.

**Remember:** autonomy should grow only as evaluation and controls prove it is safe.

## 9. Evaluation

Evaluation turns “the demo looked good” into evidence. Build a representative set of real questions, expected behaviors, difficult edge cases, and known failure examples.

Measure the system at several levels:

| Layer | Example measure |
|---|---|
| Retrieval | Relevant source appears in the top results |
| Grounding | Claims are supported by retrieved evidence |
| Tool use | Correct tool and valid arguments were selected |
| Answer quality | Correctness, completeness, clarity, and usefulness |
| Safety | Restricted requests are refused or escalated correctly |
| Operations | Latency, token usage, cost, failures, and availability |

Use code-based checks for exact requirements and model-based graders for qualities that need judgment. Review a sample of grader decisions because automated evaluators can also be wrong.

**Remember:** evaluate the full system and the individual stage that may have caused the failure.

## 10. Security and governance

AI systems add new risks because natural-language content can influence behavior. **Prompt injection** occurs when untrusted content tries to override instructions or trigger an unsafe action.

Core controls include:

- least-privilege access;
- strong authentication and role-based authorization;
- separation of instructions from untrusted content;
- input and output validation;
- allowlists for tools and arguments;
- approval before sensitive actions;
- source and tool-call logging;
- protection for secrets and personal data;
- retention rules and incident procedures.

Never rely on the model alone to enforce a permission. The application must check access before retrieving data or executing a tool.

**Remember:** authorization lives in software controls, not in the prompt.

## 11. Production and observability

A prototype becomes a product when it is repeatable, monitored, supportable, and useful. Production design should cover:

- versioned prompts, models, tools, and data sources;
- separate development, test, and production settings;
- tracing across retrieval, model calls, and tools;
- alerts for failures, latency, cost, and stale data;
- feedback and error-analysis processes;
- rollback plans and operating runbooks;
- measures tied to business outcomes.

Technical accuracy is necessary, but business value matters too. An operational analytics assistant should reduce diagnosis effort, improve consistency, shorten time to insight, or increase the number of decisions supported with evidence.

**Remember:** monitor model behavior, system health, and business impact.

## 12. Career positioning

Your strongest story connects analytics to AI-enabled decision support:

> I build governed analytics products that turn operational data into faster decisions. I use Python and structured data layers, interactive dashboards, custom GPTs and agents, retrieval, and controlled tools to help users understand what changed, which dimensions drove it, and what evidence supports the conclusion.

This is applied AI because the work combines business understanding, data quality, analytics, model interaction, workflow design, and adoption. Machine learning remains valuable for forecasting, classification, anomaly detection, and scoring; generative AI adds a natural-language reasoning and interaction layer around those capabilities.

## Quick interview distinctions

| Term | Short explanation |
|---|---|
| Prompt | Instructions and input for a model |
| Context | Everything available to the model for the current task |
| Embedding | Numeric representation used to compare meaning |
| Vector database | Storage and search designed for embeddings |
| RAG | Retrieve evidence first, then generate an answer from it |
| Tool calling | Model requests that application code run a defined function |
| MCP | Standard interface for exposing tools, resources, and prompts |
| Agent | Model-driven loop that selects steps and tools |
| Workflow | Predetermined sequence of steps and rules |
| Evaluation | Repeatable measurement of system behavior and quality |
| Guardrail | A control that constrains, checks, or routes behavior |
| Trace | Record of the steps, model calls, and tool calls in a run |

## Review questions

1. Why is a model only one part of an AI application?
2. When would you use RAG instead of fine-tuning?
3. What is the difference between an MCP tool and an MCP resource?
4. When is a deterministic workflow safer than an agent loop?
5. How would you test whether a wrong answer came from retrieval or generation?
6. Which controls must live outside the prompt?
7. Which technical and business measures would you monitor after release?

## Official references

- [OpenAI: Agents](https://developers.openai.com/api/docs/guides/agents)
- [OpenAI: Agents SDK](https://developers.openai.com/api/docs/guides/agents/sdk)
- [OpenAI: Tools](https://developers.openai.com/api/docs/guides/tools)
- [OpenAI: MCP and connectors](https://developers.openai.com/api/docs/guides/tools-connectors-mcp)
- [OpenAI: Developer learning](https://developers.openai.com/learn/agents)
- [Model Context Protocol](https://modelcontextprotocol.io/)
- [LangChain: Learn](https://docs.langchain.com/oss/python/learn)
- [LangSmith: Evaluation concepts](https://docs.langchain.com/langsmith/evaluation-types)
