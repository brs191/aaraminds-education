# Source Verification Sheet — Interview Trends Pack

**Verified:** 21 June 2026 (every URL fetched and checked)
**Bottom line:** 69 of 70 sources are real and live. One is likely fabricated (do not cite). A few need a label or link fix before you quote them out loud.

---

## 1. DO NOT CITE — one source

| Source | Problem |
|---|---|
| arXiv "Bridging Protocol and Production: Design Patterns for Deploying AI Agents with MCP" — `arxiv.org/abs/2603.13417` | The arXiv ID `2603.13417` is structurally invalid and the abstract reads as synthetic (e.g., "97M monthly SDK downloads"). Treat as **not a verifiable paper.** Never name it in an interview. If you want an authoritative MCP reference, cite the official MCP docs (`modelcontextprotocol.io`) instead. |

---

## 2. FIX BEFORE CITING — link or label is stale (content is fine)

| Source | Fix |
|---|---|
| OpenTelemetry GenAI semantic conventions — `opentelemetry.io/docs/specs/semconv/gen-ai/` | Page is now a **redirect stub**; the conventions moved to the `open-telemetry/semantic-conventions-genai` GitHub repo. Cite the GitHub repo, or cite the OTel **AI Agent Observability** blog (which is live and excellent). |
| Nasscom GCC page — `nasscom.in/gcc/` | Resolves but serves **stale 2024 conclave content**. Drop it; the Zinnov-Nasscom 2026 report and PDF are the strong, current GCC citations. |
| Google "Agentspace" — `cloud.google.com/gemini-enterprise` | URL is live but "Agentspace" is the **old name**. Google rebranded it to **Gemini Enterprise app**. Say the new name. |
| Google "Architecture Framework" — `docs.cloud.google.com/architecture/framework` | Now titled **Google Cloud Well-Architected Framework**. Use the new name. |
| OWASP Top 10 for LLM Apps 2025 | Hub page is live, but the inline list still shows v1.1 (2023). The **2025 version exists and is linked** — make sure you read the 2025 list, not the older one on the landing page. |

---

## 3. SAFE TO CITE — verified real, live, accurately titled

**AI trend reports (all genuine, correctly dated):**
- McKinsey *State of AI in 2025* (Nov 2025) and *State of AI trust in 2026: shifting to the agentic era* (Mar 2026)
- Deloitte *State of AI in the Enterprise 2026*
- Gartner — 40% of enterprise apps to feature task-specific AI agents by 2026 (press release, Aug 2025)
- Forrester *Predictions 2026: AI Moves From Hype To Hard-Hat Work* (Oct 2025)
- DORA *State of AI-assisted Software Development 2025* (live; link redirects to `/research/2025/dora-report`)

**AI governance:** NIST AI RMF · ISO/IEC 42001 · EU AI Act · CSA Agentic NIST AI RMF Profile (OWASP — see note above)

**MCP / Microsoft / OpenAI:** MCP docs + GitHub org · OpenAI Agents SDK (Tools, Tracing) · Microsoft Foundry (docs, Agent Service, observability, evaluations, MCP connection) · Microsoft Fabric (overview, OneLake, governance, Purview) · GitHub Copilot usage metrics + REST API + "Measuring Copilot success"
- *Note: "Azure AI Foundry" is now "Microsoft Foundry" — all `/azure/foundry/` links resolve and carry the new branding.*

**RAG / Search:** Lucene BM25Similarity · Azure AI Search (RAG, hybrid, BM25 scoring) · OpenSearch (hybrid, AI search) · Elastic hybrid search

**Google:** Gemini Enterprise Agent Platform (overview, product page, Agent Search, grounding, safety filters — all *formerly Vertex AI*) · SRE Book (SLOs, Embracing Risk, Monitoring) · SRE Workbook (Implementing SLOs)

**Infra / reliability:** Kubernetes · Kueue + GKE Kueue tutorial · NVIDIA GPU Operator · Ray Serve · vLLM production stack · OTel AI Agent Observability blog

**FinOps / cloud:** FinOps Framework + Unit Economics capability · AWS Well-Architected + Cost Optimization Pillar

**Databricks:** Unity Catalog (AWS + Azure) · data lineage · compute policies · build agents · Lakehouse Monitoring (*note: cited link is the SAP-edition page; AWS/Azure equivalents exist*)

**GCC:** Zinnov-Nasscom India GCC Landscape 2026 (page + PDF) · Zinnov-Indiaspora GCC AI Opportunity 2026 · Zinnov "5 Shifts" 2025 blog

---

## Interview rule of thumb
Quote a finding ("a 2025 DORA study found AI amplifies both throughput and instability") rather than reciting a URL. Name a specific report only if you've read it and can defend what it says. The one source above in red would do real damage if named — cut it now.
