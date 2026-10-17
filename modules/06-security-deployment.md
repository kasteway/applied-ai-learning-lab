# Module 6: Security, governance, and deployment

## Goal

Turn a prototype into a controlled, observable application.

## Security controls

- Classify data before using it with a model or external tool.
- Minimize the data included in prompts and tool calls.
- Store secrets outside source code.
- Authenticate users and authorize every protected operation.
- Apply least privilege to tools, databases, and service accounts.
- Validate model-generated arguments before execution.
- Require human approval for sensitive actions.
- Treat retrieved content and tool output as potentially malicious.
- Log access, tool calls, approvals, failures, and changes.
- Define retention and deletion behavior.

## Prompt-injection threat model

Ask:

1. What untrusted content enters the context?
2. What secrets or private data could the system access?
3. What tools could create external effects?
4. Can retrieved content influence tool selection?
5. Which actions require explicit approval?
6. How will suspicious requests and outputs be detected?

## Deployment checklist

- [ ] Reproducible environment or container
- [ ] Development, test, and production separation
- [ ] Secret manager and key rotation
- [ ] Authentication and role-based authorization
- [ ] Health, freshness, latency, error, and cost monitoring
- [ ] Trace and audit-log access controls
- [ ] Automated evaluation in the release process
- [ ] Incident response and rollback plan
- [ ] Documented limitations and user guidance

## Portfolio evidence

Publish an architecture diagram, threat model, evaluation report, monitoring screenshot, and runbook. Explain the controls in business language as well as technical language.

