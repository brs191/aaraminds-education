# MCP Governance Series — Part 3: The Governance Operating Model

*Three boundaries are a lens. An operating model is how the lens holds at scale.*

Part 2 ended on a promise. Three boundaries are easy to draw. Three hundred — across teams, tenants, tools, and agents — are hard to govern. This is that piece.

Start with the scenario from Part 2, because it worked.

A P1 fired on the payment service. The AI Incident Assistant queried logs through one MCP server, and every boundary it crossed had a control behind it. One server. One team. One threat model. Entirely governable.

Now move the clock forward eighteen months.

The same enterprise is running 140 MCP servers across 40 teams. Their tools reach Splunk, Datadog, Entra ID, deployment pipelines, and customer records. Some were threat-modeled. Some were stood up in a hackathon and never decommissioned. And nobody can answer the one question that decides whether any of this is governable:

How many MCP servers are in production right now, and who owns each one?

That is the real failure mode at this stage. Not a weak control on one server. A missing operating model across many.

This is not hypothetical. I have watched a team ship an MCP server during an internal hackathon to let an agent triage support tickets. It was useful, so it stayed. Within two months it had picked up a tool that could read the customer database and another that could restart a stuck service — each added by a well-meaning engineer, none threat-modeled, and the server itself in no inventory anyone could name. Nothing was breached. It simply sat in production holding credentials it should never have had, until a routine audit stumbled onto it. It was not attacked. It was never governed.

## Governance at scale is an operating model, not a security review

In May 2025, Invariant Labs published a finding against the GitHub MCP server — a well-built, widely adopted integration with thousands of stars. A researcher planted a prompt-injection payload inside a *public* GitHub issue. When a developer later asked their agent to triage issues, the agent followed the hidden instructions, walked the developer's private repositories, and exfiltrated their contents.

The server was not vulnerable. The code was fine. Invariant's conclusion is the line every engineering leader should sit with: the flaw "cannot be resolved through server-side patches. It requires architectural controls — not just software updates."

Read that again. The control that was missing did not live inside the server. It lived in the operating model around it.

A second case makes the same point from the opposite direction. CVE-2025-6514, disclosed in mid-2025, was a command-injection flaw in `mcp-remote`, a connector proxy used to reach remote MCP servers. A malicious server could run operating-system commands on the client. The package had been downloaded more than 437,000 times.

The lesson is not the CVE. It is that a widely used connector sitting inside many agent toolchains was inventoried and governed by no one. That is the definition of shadow infrastructure — and it is exactly what 140 ungoverned servers become.

So Part 3 is not another control list. It is the operating model that makes controls hold when there are too many servers for any one person to review. Four pillars.

## Pillar 1 — Registry: you cannot govern what you cannot see

Every MCP server in production is registered, with its owner, the tools it exposes, the downstream systems it reaches, the identities and scopes it carries, its threat-model status, and the date it was last reviewed.

`mcp-remote` is the argument for this pillar. An unregistered component is an ungoverned one, and you only discover it during the incident.

On Azure, the registry is not a spreadsheet. It is a system of record — Azure API Center as the catalog, populated by CI/CD at deploy time so registration is automatic rather than voluntary. A server that is not in the registry does not receive a managed identity and cannot reach production systems.

**Governance gate:** *If it is not in the registry, it is not in production.*

## Pillar 2 — Ownership: federate accountability, standardize the pattern

The product team that builds an MCP server owns it — its threat model, its tool scopes, its incident response. The central platform team owns the *pattern*: the reference architecture, the approved auth model (Microsoft Entra ID, scoped OAuth, managed identity), the redaction library, and the pipeline that registers and deploys.

This split is deliberate. Centralizing ownership of every server is how you manufacture a bottleneck — and a bottleneck is how you breed shadow MCP, because blocked teams route around the gate.

Federate the ownership. Standardize the pattern.

**Governance gate:** *Every server has one accountable owner and conforms to one platform pattern.*

## Pillar 3 — Cadence: review on triggers, not the calendar alone

A threat model is not a launch artifact you file once and forget. It is re-reviewed when risk actually changes: a new tool added to a server, a scope change on any tool, or a new downstream system. Plus a baseline quarterly review for everything in the registry.

The first three are event-driven and non-negotiable, because they are the precise moments the boundary shifts. The quarterly review catches the drift that no single event triggered.

On Azure, wire the triggers into the pipeline. A pull request that adds a tool or widens a scope blocks merge until the threat model is updated. Cadence enforced by process, not by good intentions.

**Governance gate:** *What change just happened, and did it trigger a re-review?*

## Pillar 4 — Evidence: runtime proof, not point-in-time approval

Approval at launch proves intent. Only runtime evidence proves control.

The minimum bar is tool-call lineage: every invocation traceable to identity, session, tool, and the downstream action it caused. Not API logs — execution lineage.

On Azure, that is OpenTelemetry spans emitted from the MCP server into Azure Monitor and Log Analytics, correlated by session and agent identity, so you can reconstruct why an agent reached for a tool and what it touched.

Without lineage, you cannot answer the post-incident question. And in agentic systems, the post-incident question is always the same: what influenced this execution?

**Governance gate:** *Can we reconstruct any tool call, end to end, after the fact?*

## How a leader measures it

Three metrics, each anchored to a baseline so the numbers mean something. The baseline is not an industry benchmark — it is the number you get the first time you measure your own estate. Write it down; that is the line you are improving from.

**Registry coverage** — the percentage of production MCP servers with a registered owner and a current threat model. Measure it on day one. Most teams are unpleasantly surprised by how much of their estate is shadow or unknown-owner. Set the target explicitly: 100% registered with a named owner within two quarters, and treat the gap as a backlog with owners and dates, not a statistic.

**Mean time to re-review** — the elapsed time from a triggering change to an updated threat model. Measure it in days, not quarters. If a scope change takes longer than one sprint to re-review, the trigger fired but the cadence did not — that is the gap to close. Target: re-review inside the sprint the change ships.

**Lineage completeness** — the percentage of production tool calls with a full end-to-end trace. Measure it honestly; most teams can trace far less than they assume. Target 100%, and treat anything below as named gaps with owners, not a rounding error. The number you cannot afford is the one you never measured.

No vanity numbers. Every one starts from a real baseline and moves toward a stated target.

## The anti-pattern to kill: the central-team bottleneck

The most common way enterprises try to govern MCP at scale is to route every new server through a central security team for approval. It feels safe. It fails twice.

It does not scale — the review queue becomes the constraint on every AI initiative in the company. And it backfires — teams blocked by the queue quietly stand up their own unregistered servers, and now you have manufactured the exact shadow-MCP problem you were trying to prevent. The `mcp-remote` pattern, but self-inflicted.

The platform team's job is not to approve every server. It is to make the governed path the easy path — a paved road where registration, identity, redaction, and lineage are defaults, not paperwork.

Govern the pattern. Not every instance.

## Closing

The Three-Boundary Model showed where execution crosses trust. The Governance Operating Model is how that discipline survives contact with forty teams and a hundred servers.

Registry makes it visible. Ownership makes it accountable. Cadence keeps it current. Evidence proves it is real.

The architecture was the lens. This is how the lens holds.

---

### Next in the Series — Part 4

**The Paved Road: A Reference Architecture for Governed MCP on Azure**

An operating model tells teams what good looks like. A paved road makes good the default.

Part 4 is the architecture underneath this model — how identity, scoping, redaction, and lineage ship as platform primitives, so a team earns governance by building on the road instead of reading a policy.

#MCP #AIGovernance #EnterpriseArchitecture #PlatformEngineering #AILeadership

---

<!--
AUTHOR NOTES — delete before posting

Read time: ~6 min (~1,450 words).

IMPORTANT — the opening vignette ("I have watched a team ship an MCP server during an internal hackathon…")
is a TEMPLATE. Before publishing, replace its specifics with a real, anonymized incident you have actually
seen, or confirm it genuinely matches one. Do not ship a fabricated personal anecdote under your byline.
If you have no comparable experience, change "I have watched" to "I have seen teams do this" or recast it
as a composite ("a pattern I have seen repeatedly").

Metrics note: the targets (100% registry coverage within two quarters; re-review inside the sprint; 100%
lineage) are deliberately prescriptive, not sourced statistics. The "baseline" is the reader's own day-one
measurement — by design, so nothing here is an unverifiable industry number.

Sources for the two incidents (link or footnote as you prefer):
- GitHub MCP prompt injection (Invariant Labs, May 26 2025): https://invariantlabs.ai/blog/mcp-github-vulnerability
  Key quote used: "cannot be resolved through server-side patches. It requires architectural controls — not just software updates."
- mcp-remote CVE-2025-6514 (OS command injection, CVSS 9.6, 437k+ downloads; discovered by JFrog Security Research, 2025; fixed in v0.1.16):
  https://jfrog.com/blog/2025-6514-critical-mcp-remote-rce-vulnerability/  (NVD: https://nvd.nist.gov/vuln/detail/CVE-2025-6514)

Suggested cover visual: same MCP Governance Series template. Center concept = "Governance Operating Model"
with four pillars across the base: REGISTRY / OWNERSHIP / CADENCE / EVIDENCE. Keep the three-boundary
icons from Part 2 small in the top band to signal continuity (lens -> operating model).

Verify before posting: confirm Azure API Center is still your intended catalog of record and that your
team uses Entra ID + managed identity as the standard auth pattern (matches your stack; swap names if not).
-->
