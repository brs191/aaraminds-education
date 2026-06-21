# MCP Servers Do Not Need More Trust. They Need Better Boundaries.

*AaraMinds - enterprise AI leadership for operators who need judgment, not hype.*

**Why threat modeling is becoming the first production discipline for agentic systems.**

---

Most MCP conversations start in the wrong place.

"Which tools should we expose to the agent?"

It sounds practical. It is also incomplete.

The better question is: **what new trust boundary are we creating, and who is allowed to cross it?**

That difference matters.

An MCP server is not just a connector. It is a system that receives tool calls influenced by text the enterprise may not control, acts on downstream systems with real permissions, and returns results that may shape the model's next action.

That is not ordinary integration plumbing.

That is a privileged decision surface.

## The Thesis

MCP server security should start with threat modeling, not guardrail shopping.

Runtime controls are necessary: authentication, authorization, input validation, rate limits, redaction, audit logs, and evaluation gates. But these controls only work when the team has first named the boundary they are defending.

Without a threat model, guardrails become scattered features.

With a threat model, every control has a job.

## Why This Matters Now

MCP has moved quickly from developer convenience to agent infrastructure.

That shift changes the security posture.

The latest MCP authorization specification, dated 2025-11-25, treats remote MCP authorization as a serious OAuth problem. It requires OAuth 2.1 support by authorization servers and requires MCP servers to publish OAuth protected resource metadata so clients can discover the right authorization server and scopes.

The MCP security guidance now explicitly warns about confused deputy flows, token passthrough, SSRF through OAuth metadata discovery, session hijacking, local server compromise, and scope minimization.

OWASP has also published an MCP Top 10, with risks that map directly to enterprise operating failures: token exposure, scope creep, command injection, missing auditability, shadow MCP servers, and context over-sharing.

The research signal is moving in the same direction. MCPTox, an arXiv benchmark published in August 2025, tested tool poisoning against 45 live MCP servers and 353 real tools. Its authors reported that malicious instructions embedded in tool metadata can cause agents to perform unauthorized actions through otherwise legitimate tools.

And this is no longer only a research concern. NVD published CVE-2026-5058 in April 2026 for an `aws-mcp-server` command injection remote code execution vulnerability, where improper validation before a system call allowed unauthenticated remote code execution.

The pattern is clear enough for leaders to act on:

MCP is not merely an integration standard. It is becoming an execution layer for AI agents.

Execution layers need threat models.

## The Three-Boundary Model

The most useful way to review an MCP server is not by listing its tools.

It is by walking the three boundaries it creates.

```text
Attacker-influenced text
        |
        v
LLM / agent  --->  MCP server  --->  downstream systems
        ^              |
        |              v
        +------ tool output returns to the model
```

### Boundary 1: The Agent Calls the MCP Server

This is where tool calls arrive.

Some calls are legitimate. Some are influenced by hostile content the model has read in a document, issue, log line, web page, email, or repository file.

The threats here are familiar but sharper in an agentic system:

- spoofing: the caller is not who the server thinks it is
- tampering: inputs are shaped to bypass validation
- denial of service: oversized or pathological inputs exhaust the server
- scope creep: an agent gets access to tools beyond the work it should perform

The controls belong at the boundary:

Authenticate the caller. Authorize per tool. Validate every input with strict schema, size, pattern, and depth limits. Rate-limit by identity and tool. Reject broad "manage everything" tools in favor of one action per tool.

Do not confuse authentication with authorization.

An authenticated agent should still be unable to invoke a tool it does not need.

### Boundary 2: The MCP Server Acts on Downstream Systems

This is where the risk becomes material.

The server may query logs, read repositories, update tickets, restart deployments, inspect cloud resources, retrieve customer data, or call internal APIs.

The agent does not need direct access to the crown jewels. It only needs a tool that has that access.

The threats here are elevation of privilege, tool composition abuse, command injection, cross-tenant leakage, and repudiation.

The controls are not glamorous:

Use least-privilege service identities. Bind downstream access to the user's entitlement, not only to the server's identity. Make destructive tools dry-run by default. Require human approval for high-impact write paths. Keep audit records with user, tool, arguments, authorization decision, downstream target, outcome, and correlation ID.

The hard part is composition.

One tool may safely read logs. Another may safely deploy code. Together, they may create an exfiltration or takeover path if the first tool leaks a token and the second tool accepts it.

Threat modeling is how that chain becomes visible before production.

### Boundary 3: Tool Output Returns to the Model

This is the boundary many teams under-design.

A tool output is not just a response. It becomes part of the model's context.

If the output contains secrets, PII, internal URLs, access tokens, stack traces, customer data, or attacker-controlled instructions, it can shape the next step in the workflow.

The threats are information disclosure, output-as-instructions, context injection, and over-sharing.

The controls are structural:

Return structured data, not loose prose. Redact secrets before returning and before logging. Whitelist fields instead of returning whole records. Mark tool output as data in the client framing. Avoid sharing persistent context across users, tenants, or unrelated tasks unless the isolation model is explicit.

This is where the operating principle matters:

Do not defend the content of untrusted text with fragile pattern matching alone.

Defend the boundary: structure, scope, redaction, provenance, and approval.

## The Operating Model: Threat Model First, Guardrails Second

A practical MCP review can be simple.

For every server, inventory:

- each tool, resource, and prompt
- its risk tier: informational, read, write, destructive
- caller identity required
- downstream permissions used
- input schema and limits
- output shape and sensitive fields
- audit event emitted
- re-review trigger

Then apply STRIDE to each tool:

```text
Spoofing: who can pretend to be a valid caller?
Tampering: what input can alter behavior?
Repudiation: can the user deny the action later?
Information disclosure: what can leak?
Denial of service: what can exhaust the system?
Elevation of privilege: what action exceeds the caller's rights?
```

Then add the MCP-specific layer:

```text
Tool poisoning: can metadata influence the model into unsafe calls?
Output-as-instructions: can returned data steer the next model action?
Tool composition abuse: can two safe tools become unsafe together?
Shadow MCP: can teams deploy servers outside governance?
Context over-sharing: can one task, tenant, or user leak into another?
Supply-chain compromise: can a server, package, image, or registry entry be swapped?
```

Only after this pass should the team finalize the guardrail chain:

```text
validate -> authorize -> rate-limit -> audit -> execute -> redact -> observe
```

Same chain.

Every tool.

No exceptions for "read-only" tools until their output has been classified.

## A Concrete Enterprise Example

Consider a `query_logs` MCP tool.

Internally, it feels low risk. It only reads logs.

Then the server moves from local stdio use to remote HTTP so a support agent can diagnose incidents across services.

The threat model changes immediately.

Boundary 1 now requires OAuth-based authentication, per-tool authorization, tenant-aware access, and rate limits. The old assumption that "only the local developer can call it" no longer applies.

Boundary 2 requires least-privilege access to the logging backend. The tool should not read every workspace because the server identity can.

Boundary 3 is the real finding. Logs often contain bearer tokens, emails, customer identifiers, stack traces, internal hostnames, and sometimes complete request payloads.

So the highest-value control is not a dashboard.

It is output redaction before the result returns to the model.

The tool may still be read-only. The output is not low risk.

## Two Leadership Mistakes

The first mistake is treating the MCP server as engineering plumbing.

That leads to local experiments becoming production dependencies without ownership, inventory, review, or audit. OWASP's "shadow MCP servers" risk is not theoretical in this sense. It is the predictable outcome of making tool connection easier than tool governance.

The second mistake is treating evaluations as runtime security.

Evals are valuable. They help detect regressions and measure behavior. But an eval suite does not stop an unsafe tool call in the moment. It does not redact a token from a response. It does not enforce a user's permission against a downstream system.

Evals belong in the operating model.

They are not a substitute for boundary controls.

## What Senior Leaders Should Ask

For every MCP server moving beyond local experimentation, ask five questions:

1. What tools does this server expose, and what is each tool allowed to do?
2. Which identity is used at each boundary: user, agent, server, downstream system?
3. What can the server return to the model, and how is sensitive output redacted?
4. Which tool calls require human approval, and where is that enforced?
5. What change triggers threat-model re-review: new tool, new transport, new tenant, new data class, new write path, or new downstream permission?

The governance gate should be equally plain:

No shared or remote MCP server ships without a written threat model, a tool inventory, per-tool authorization, output redaction, audit logging, and a re-review trigger.

This is not bureaucracy.

It is the minimum operating discipline for giving AI systems access to real work.

## Closing

MCP makes agents useful because it gives them tools.

The same fact makes MCP risky.

The enterprise question is not whether to use MCP. The question is whether each MCP server is treated as a governed trust boundary or as an invisible connector.

Invisible connectors become incidents.

Governed boundaries become platforms.

Threat model first.

Then the guardrails know what they are defending.

---

## Source Note

- Model Context Protocol, `2025-11-25` authorization specification: OAuth 2.1, protected resource metadata, authorization server discovery, incremental scopes.
- Model Context Protocol security best practices: confused deputy, token passthrough, SSRF, session hijacking, local MCP compromise, scope minimization.
- OWASP MCP Top 10 `v0.1`: token exposure, scope creep, command injection, insufficient auditability, shadow MCP servers, context injection and over-sharing.
- NSA, *Model Context Protocol (MCP): Security Design Considerations*, May 2026: dynamic tool invocation, implicit trust relationships, context sharing, and implementation rigor for production environments.
- Wang et al., *MCPTox: A Benchmark for Tool Poisoning Attack on Real-World MCP Servers*, arXiv, August 2025.
- NVD, CVE-2026-5058, published April 10, 2026: command injection remote code execution in `aws-mcp-server`.

## Links

- https://modelcontextprotocol.io/specification/2025-11-25/basic/authorization
- https://modelcontextprotocol.io/specification/2025-11-25/basic/security_best_practices
- https://owasp.org/www-project-mcp-top-10/
- https://www.nsa.gov/Portals/75/documents/Cybersecurity/CSI_MCP_SECURITY.pdf
- https://arxiv.org/abs/2508.14925
- https://nvd.nist.gov/vuln/detail/CVE-2026-5058

Notes:
- Verification needed: Before publication, confirm whether AaraMinds wants to cite NSA guidance directly in a LinkedIn newsletter; it strengthens authority but changes the tone from operator essay to security briefing.
- Optional visual: A simple three-boundary diagram: Agent -> MCP Server -> Downstream Systems, with output returning to Agent and controls attached to each boundary.
- Suggested next edit: Create a shorter companion LinkedIn post that opens with "An MCP server is not an integration. It is a trust boundary."
