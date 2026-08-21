# MCP Governance Series — Part 5: Bringing Shadow MCP onto the Paved Road

*A brownfield migration playbook for the MCP servers, tools, and integrations you already have.*

The paved road is easy to describe when everything is new. Most enterprises will not start there.

They will start with what already exists: local MCP servers built during experiments, wrappers around internal APIs, hackathon tools that quietly became useful, developer scripts exposed to agents, team-built connectors with no clear owner, and proofs of concept that were never formally promoted but now have production reach.

That is shadow MCP — rarely malicious, rarely careless, usually just teams moving faster than the governance model.

The scale is no longer hypothetical. Within a year of the protocol's late-2024 debut, more than 10,000 public MCP servers had appeared, and Qualys has called MCP servers "the new shadow IT for AI."

That framing matters. Shadow MCP is not only about the servers teams intentionally publish. It is also about the local tools, unregistered wrappers, unofficial connectors, and production-adjacent experiments that never entered an enterprise inventory.

Part 4 described the paved road for governed MCP: registry, gateway, scoped identity, CI/CD enforcement, content safety, observability, and evidence.

Part 5 is the harder problem — moving existing capability onto that road without killing the adoption that produced it.

The operating principle:

> Do not punish MCP adoption. Make it visible, scoped, and governable.

Shadow MCP is not solved by saying no. It is solved by giving useful capability a governed path.

## Why this is a migration problem

Enterprise governance is usually built for new work: new APIs get design review, new services get architecture review, and new deployments pass release gates.

AI tooling often enters through a different door. Someone stands up a local MCP server to query logs. Someone wraps an internal API. A hackathon project proves useful and quietly stays. None of it looks like enterprise infrastructure on day one.

The risk changes when the tool gains reach — into production logs, repositories, tickets, customer records, identity systems, and deployment pipelines.

A common pattern looks like this: a support-triage server starts as an internal experiment, then gains a tool to read customer data and another to restart a stuck service. Nothing is breached. No one intended to bypass governance. But the server is now holding credentials and production reach it was never designed to govern. That is the canonical shape of shadow MCP.

Security reports through 2025–2026 point in the same direction: unmanaged MCP servers can expose weak authentication, broad credentials, and limited runtime visibility. One critical example, CVE-2026-33032, was recorded as a missing-authentication vulnerability affecting an MCP path in Nginx UI. The unmanaged surface is not just wide. It can become operationally material before the enterprise sees it.

So the question is no longer whether teams should experiment with MCP. It is: which existing servers already cross trust boundaries, and how quickly can the useful ones move onto the governed path?

That is a migration problem. Not a policy problem alone. Not a security review alone. Not a documentation exercise on its own. It is a migration problem.

## The Shadow MCP Migration Playbook

The goal is to separate useful capability from unmanaged risk. Six moves do that: **Discover, Classify, Contain, Onboard, Retire or Refactor, Prove.**

It is not a waterfall. Some servers move quickly from discovery to onboarding. Some need immediate containment. Some should be retired. A few should be rebuilt. The discipline is knowing which is which.

*[IMAGE — Diagram 1 of 2: Shadow MCP Inventory Map]*

### 1. Discover — find what already exists

The first governance problem is not control. It is visibility. Most shadow MCP never entered an enterprise inventory because it was never treated as infrastructure.

Discovery has to reach wider than a catalog: developer workstations, source repositories, CI/CD pipelines, agent-platform configurations, MCP client files, API wrappers, internal tools, team-owned services, and hackathon repositories.

There is now an official MCP Registry preview, with a direction toward private enterprise sub-registries. Azure API Center can also act as the enterprise system of record for MCP inventory and discovery. That direction is useful, but it does not solve the brownfield problem by itself. A registry lists what someone chose to publish; shadow servers, by definition, may not be there. Discovery has to reach past the catalog to where servers actually run.

For each server, capture more than its address: which tools it exposes, which systems it reaches, which identities it uses, which agents can invoke it, and which teams depend on it.

A server that searches internal documentation is one concern. A server that queries production logs is another. A server that can update tickets or trigger a deployment is a different category entirely. The output is a first inventory — not remediated, just visible enough to make decisions.

**Governance gate:** Do we know which MCP servers exist, and which already have production reach?

### 2. Classify — separate experiment from exposure

Once the inventory exists, the next mistake is treating everything the same. Sort servers into practical groups:

- **Experiment** — local, no production reach, few users.
- **Production-adjacent** — touches production data, logs, tickets, repositories, or workflows, but is not yet governed.
- **Production integration** — actively used by agents or workflows with real operational dependency.
- **High-risk unmanaged** — broad scopes, unclear ownership, sensitive access, write or destructive capability, shared credentials, or weak visibility.
- **Duplicate or obsolete** — replicates existing capability, has no clear owner, or is no longer needed.

This is where the Part 4 policy tiers earn their keep. A read-only documentation lookup does not carry the migration urgency of deployment control. Sensitive read requires scoped identity, output redaction, and lineage. Write or destructive tools require approval paths, rollback plans, and stronger evidence.

Classification answers one question: what is the smallest safe next step for this server?

**Governance gate:** Is this an experiment, production-adjacent, production-critical, high-risk, or ready to retire?

### 3. Contain — reduce risk without stopping useful work

Containment is where enterprises overcorrect. They find shadow tooling and freeze everything. That feels safe, but it teaches teams that governance arrives late and takes useful tools away.

Risk-based containment is better. It can include rotating exposed secrets, removing broad scopes, disabling write or destructive tools, moving traffic behind the gateway, forcing read-only until review, logging tool calls before full onboarding, and requiring named ownership.

Some servers should stop immediately: unknown owner, production write access, shared credentials, sensitive-data exposure, no logs, destructive capability, or untrusted external reach. Many others can keep running under containment while they move toward the road. Good governance does not confuse control with interruption.

**Governance gate:** What risk must be reduced before this server continues operating?

### 4. Onboard — move useful servers onto the paved road

Useful capability should be formalized, not punished. Onboarding is the Part 4 path applied to a server that already exists: register it and give it a named owner, classify each tool by risk tier, replace shared secrets with scoped workload identity, route it through the gateway, apply policy and approval rules, keep content safety at the gateway and secret / PII redaction in the server, emit telemetry into the evidence layer, and define re-review triggers.

The rule that keeps onboarding humane: do not make it feel like a rebuild unless a rebuild is actually needed. Many servers reach the road by changing identity, routing, registration, logging, and policy — not by starting over.

**Governance gate:** What must change for this server to become governed without rebuilding unnecessary parts?

### 5. Retire or Refactor — do not preserve bad patterns

Not everything discovered deserves onboarding. Retire what is unused, duplicative, ownerless, unobservable, or no longer tied to a real use case.

Refactor servers that expose useful capability behind weak boundaries: split read tools from write tools, narrow downstream permissions, move redaction into the response path, standardize structured outputs, add tool-level authorization, separate tenant and environment scopes, and replace local credentials with managed identity.

The goal is not to preserve every artifact of experimentation. It is to preserve useful capability in a form the enterprise can govern.

**Governance gate:** Is this worth governing as-is, or should the capability be retired, merged, or rebuilt?

### 6. Prove — show that invisible execution is shrinking

This is the move most brownfield programs get wrong, and the one that matters most. Cleanup can manufacture a false sense of progress: workshops happen, inventories start, standards get published, a few servers are remediated — and none of that proves the enterprise is safer.

The only proof that counts is whether unmanaged execution is measurably shrinking. Anchor it to the metrics from Parts 3 and 4 — registry coverage, lineage completeness, and mean time to re-review — then add two brownfield signals: production-reaching servers still outside the gateway, and the trend line of known shadow servers.

Measure risk reduction, not governance activity. More reviews, more registered servers, and more approvals mean nothing while lineage stays incomplete and identities stay broad.

**Governance gate:** Can we prove that shadow MCP is shrinking and governed coverage is rising?

*[IMAGE — Diagram 2 of 2: Shadow MCP Migration Playbook]*

## What leaders should watch

Brownfield migration fails in four predictable ways. Treat all shadow MCP as misconduct, and teams go underground. Treat all of it as innovation, and unmanaged execution hardens into infrastructure. Build a heavy review process before a usable paved road, and the shortcut stays more attractive than the governed path. Measure activity instead of risk, and you will report progress while the exposure is unchanged.

The through-line is the same one from Part 4: the platform team's job is not to approve every server. It is to make the governed path the easy one — and to keep useful MCP capability from becoming invisible infrastructure.

## Closing — do not punish adoption

Shadow MCP is a signal. It tells you teams found something useful before the enterprise built the governed path. That does not make every server safe or every shortcut defensible. But it means governance has to answer with more than a stop sign.

The order is what makes brownfield governable: discover before you control, classify before you migrate, contain before you scale, onboard before you expand, and prove before you declare success. Start with punishment and teams hide. Start with control before discovery and you govern only what you already knew about.

Part 1 argued that MCP servers are trust boundaries. Part 2 showed where those boundaries are crossed. Part 3 introduced the operating model. Part 4 made it executable as a paved road. Part 5 is the migration from useful-but-unmanaged capability into governed execution.

Do not punish MCP adoption. Make it visible, scoped, and governable.

For teams inventorying an existing MCP estate, the discover-and-classify pass is usually where the real exposure surfaces.

---

**Next in the Series — Part 6: AI Runtime Governance Becomes a Platform Discipline.** MCP governance is the entry point. The larger shift is the control plane for agents, tools, identity, approvals, context return, observability, and evidence. Part 6 closes the series by connecting MCP governance to the platform discipline enterprises will need for agentic systems.

*Sources: "New shadow IT for AI" framing and 10,000+ public-server figure — Qualys, 2026. CVE-2026-33032 missing-authentication reference — NVD, 2026. Official MCP Registry preview and private sub-registry direction — Model Context Protocol blog, 2025. Azure API Center / API Management MCP support — Microsoft Learn, current as of July 2026.*

\#MCP #AIGovernance #EnterpriseArchitecture #PlatformEngineering #AILeadership
