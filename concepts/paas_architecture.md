============================================================
Microsoft-centric Cloud PaaS Architecture Patterns — Quick Ref (Advanced Additions)
============================================================
Adds: multi-tenant patterns, API governance, eventing guarantees, data contracts,
confidential computing options, advanced networking, SRE/SLO, and platform ops.

-----------------------------
0) Platform Engineering Add-ons
-----------------------------
- Environment strategy: dev/test/prod + ephemeral preview envs per PR (where feasible)
- Golden paths: templates for “API + messaging + DB + observability” with IaC modules
- Policy as code: Azure Policy initiatives + exemptions workflow + drift detection
- Release patterns: canary + feature flags (App Config), progressive exposure (Front Door rules)

-----------------------------
1) Identity & Auth (Advanced)
-----------------------------
- Workload identity:
  - Preferred: Managed Identity for Azure resources
  - Kubernetes: Workload Identity (AKS) / federated credentials (no secrets)
- Auth patterns:
  - B2E: Entra ID (OIDC) + group/app-role claims
  - B2C/B2B: Entra External ID / B2C (if customer-facing identity needed)
- Token validation: centralize at APIM/Front Door when possible; still validate in apps for defense-in-depth
- Key rotation: Key Vault + automated cert rotation; avoid long-lived client secrets

-----------------------------
2) Networking & Edge (Advanced)
-----------------------------
- Edge routing:
  - Front Door for global anycast + WAF + multi-region failover
  - App Gateway WAF for regional, VNet-centric patterns
- Private-only PaaS:
  - Private Link + “disable public network access” for Storage/SQL/Cosmos (where supported)
  - Use Private DNS Zones + centralized DNS forwarding
- Egress governance:
  - Azure Firewall with FQDN tags + TLS inspection (where mandated)
  - NAT Gateway for stable outbound IPs (partner allowlists)
- Segmentation:
  - Separate inbound, app, and data subnets; NSGs as defense-in-depth (even with PaaS)
- Multi-region connectivity:
  - Virtual WAN for branch connectivity; hub-spoke for simpler estates

-----------------------------
3) API Platform & Governance (Advanced)
-----------------------------
- APIM roles:
  - North-south gateway: external APIs, policies, developer portal
  - East-west gateway: internal APIs, mTLS, routing, rate limiting
- Policies (common enterprise must-haves):
  - JWT validation, quota/rate limit, IP allowlist, header injection (correlation)
  - Request/response transformation (careful: avoid heavy transforms at gateway)
  - Versioning: URL (/v1), header, or query param; deprecation policy + sunset headers
- Observability:
  - Standard correlation headers: traceparent + x-correlation-id
  - Log request IDs + consumer/app IDs + subscription IDs in APIM

-----------------------------
4) Messaging Guarantees & Workflow Patterns (Advanced)
-----------------------------
- Exactly-once is rare: design for at-least-once + idempotent consumers
- Service Bus advanced:
  - Sessions: ordered processing per key (per user/tenant/entity)
  - Duplicate detection (time-window) for producer-side help (still keep consumer idempotent)
  - DLQ strategy: classify faults (transient vs permanent), automate re-drive tooling
- Event Grid advanced:
  - Event schema standardization (CloudEvents where practical)
  - Retry + dead-letter to storage for undeliverable events
- Orchestration:
  - Durable Functions for code-first saga/orchestration with replay-safe activities
  - Logic Apps for connector-heavy workflows + approvals + B2B
- Outbox + Inbox:
  - Outbox: DB transaction writes event to outbox table; publisher relays to bus
  - Inbox: consumer stores processed message IDs to ensure idempotency

-----------------------------
5) Data Contracts, Schema & Governance (Advanced)
-----------------------------
- Data contracts:
  - Define schema, required fields, semantics, owner, SLA, versioning rules
  - Enforce at ingestion (schema validation) + monitor drift (new/removed columns)
- Versioning:
  - Backward compatible changes as default; breaking changes require new topic/version
- Storage formats:
  - Lake: Parquet/Delta; partitioning strategy by date/tenant/source
  - Gold: curated star schemas / wide tables for BI, plus semantic models
- Quality gates:
  - “Bronze is append-only” + immutability
  - Silver: dedupe + standardize + validation; quarantine bad records
  - Great Expectations-style checks (or equivalent) + publish quality metrics

-----------------------------
6) Multi-tenant SaaS Patterns (Advanced but practical)
-----------------------------
- Tenant isolation models:
  - Shared DB + tenant_id (cheapest; strict RBAC + row-level security)
  - DB-per-tenant (strong isolation; higher ops/cost)
  - Hybrid: shared for small tenants, dedicated for large (enterprise)
- Tenant routing:
  - Tenant context from JWT claim -> route to correct data partition / connection string
- Rate limiting:
  - Per-tenant quotas in APIM + service-level throttles; protect noisy neighbors
- Data protection:
  - Per-tenant encryption keys (advanced) when required by compliance

-----------------------------
7) Reliability Engineering (SRE-level)
-----------------------------
- SLOs first:
  - Define SLI: availability, latency, error rate, freshness (data), backlog (queues)
  - Alerts based on error budget burn, not raw CPU
- Resilience patterns:
  - Bulkheads: separate thread pools per dependency or workload class
  - Timeouts everywhere (HTTP, DB, queue handlers)
  - Circuit breakers + retry budgets (avoid retry storms)
- Backpressure:
  - KEDA/autoscale on queue depth (Functions/Container Apps)
  - Shed load on non-critical endpoints when dependencies degrade
- Chaos-lite:
  - Regularly test failover of dependencies (simulate downstream outage)

-----------------------------
8) Secrets, Keys, and Confidential Options
-----------------------------
- Key Vault usage:
  - Central keys/certs; app secrets only when unavoidable
  - Access via Managed Identity; rotate automatically
- Confidential computing (when mandated):
  - Confidential VMs or confidential containers (use case dependent)
  - Always start with threat model + compliance requirement, not trend

-----------------------------
9) Operational Excellence (Enterprise)
-----------------------------
- Runbooks + playbooks:
  - DLQ handling, replay procedures, incident triage, rollback
- Audit trails:
  - Immutable storage for critical logs/events (when required)
- Change management:
  - Schema changes, API deprecations, topic migrations with comms + timelines
- FinOps:
  - Cost per tenant/request/pipeline run; budgets + anomaly detection
  - Reserved capacity for steady workloads; autoscale for burst

-----------------------------
10) Advanced “Reference Blueprints”
-----------------------------
A) Global API (multi-region active-active)
- Front Door (WAF) -> APIM (regional) -> App Service/Container Apps (multi-AZ)
- Data: Cosmos DB multi-region or SQL w/ geo-replication (depending on needs)
- Events: Event Grid regional + Service Bus (geo-dr / paired region plan)
- Observability: central workspace + distributed tracing across regions

B) Event-driven Enterprise Integration (robust)
- Producers -> Service Bus topics (sessions by entity/tenant) -> consumers
- Outbox publisher for DB consistency
- DLQs + re-drive + replay tooling + incident dashboards

C) Data Freshness Pipeline (15-min SLA)
- CDC/Events -> Stream ingest (Event Hubs) -> processing (Functions/Container Apps)
- Lake Silver/Gold updates + serving store + freshness metrics + alerting

------------------------------------------------------------
If you want, tell me which scenario you’re targeting (API platform,
integration hub, analytics pipeline, or SaaS multi-tenant) and I’ll
condense this into a single “canonical design” with:
- minimal service set
- security posture (private endpoints vs public edge)
- SLOs + core alerts
- data + event contract checklist
------------------------------------------------------------
