
OPS Data Data Platform Internal Architecture + OSI Best Practices
Scope: Azure/Microsoft Fabric-centric enterprise data platform (ingest -> lake/lakehouse -> serving -> BI),
with OSI-layer guidance for security, connectivity, and operations.

Core stack (typical):
- Ingestion/Orchestration: Azure Data Factory (ADF) / Fabric Data Pipelines
- Streaming: Event Hubs (telemetry/streams), Event Grid (events), Service Bus (commands/queues)
- Storage/Lake: ADLS Gen2 (OneLake in Fabric context), Delta/Parquet
- Processing: Fabric (Spark/Lakehouse) / Synapse / Databricks (org-dependent)
- Serving: Azure SQL / Fabric Warehouse / Synapse SQL
- Semantic/BI: Power BI
- Governance: Microsoft Purview
- Security/Monitoring: Entra ID, Key Vault, Defender for Cloud, Azure Monitor/App Insights

--------------------------------------------------------------------
1) Reference Architecture (PaaS-first, modern best practice)
--------------------------------------------------------------------
A) Sources
- SaaS (Dynamics, ServiceNow, etc.), on-prem DBs, files, APIs, IoT/telemetry

B) Ingestion Layer
- Batch: ADF / Fabric Pipelines (copy + transforms) -> Bronze
- CDC: source CDC -> landing -> Bronze (or event stream)
- Streaming: Event Hubs -> stream processing -> Bronze/Silver

C) Lakehouse Layers (Medallion)
- Bronze: raw/append-only, immutable, source-aligned partitions
- Silver: cleaned/standardized, deduped, schema-enforced, conformed dimensions
- Gold: curated business entities / aggregates for analytics + BI serving

D) Serving
- Warehouse/SQL: Fabric Warehouse / Synapse / Azure SQL for BI-friendly consumption
- APIs: lightweight serving via App Service/Functions if operational consumers need data products

E) Governance/Operations
- Purview for catalog + lineage + classification
- Monitor + Log Analytics for pipeline/run telemetry, freshness, failures
- Data quality gates + quarantine + replay tooling

--------------------------------------------------------------------
2) OSI Model Best Practices (Enterprise Data Platform Lens)
--------------------------------------------------------------------

Layer 1 (Physical) — Region/Zone separation reality
- Prefer zone-redundant PaaS where supported (resilience without IaaS).
- Define DR tier (backup/restore vs warm standby) for: lake, warehouse, critical metadata.
- Align data residency/compliance to region selection.

Layer 2 (Data Link) — Subnet segmentation & private access building blocks
- Use dedicated subnets for private endpoints (data plane services).
- Use NSGs for defense-in-depth (even though PaaS is managed).
- Avoid assumptions about L2 adjacency; treat everything as routed.

Layer 3 (Network) — Private-first data plane
- Default: Private Link for Storage/ADLS, SQL, Cosmos, Synapse/Fabric endpoints where supported.
- Central DNS: Private DNS Zones + DNS forwarding; prevent “accidentally public” resolution.
- Hub-spoke (or Virtual WAN) to centralize routing and security controls.
- Egress control: NAT Gateway for stable outbound IPs; Azure Firewall for governed egress.
- Disable public network access on data stores where possible; enforce via Azure Policy.

Layer 4 (Transport) — Throughput, reliability, and timeouts
- Use TLS everywhere; enforce minimum TLS versions via policy where possible.
- Explicit timeouts for:
  - API calls in pipelines
  - JDBC/ODBC connections
  - message consumers (poll loops)
- Connection pooling for SQL endpoints; avoid connection storms from parallel pipeline steps.
- Plan SNAT/egress scale (NAT Gateway) for high-concurrency ingestion.

Layer 5 (Session) — Auth/session semantics (tokens, long-lived connections)
- Managed Identity for pipelines/compute; avoid secrets.
- Token refresh correctness (long-running Spark jobs, large copy activities).
- For messaging: ordering/consistency at app layer (Service Bus sessions) when needed.
- Avoid sticky session assumptions; pipeline runners should be stateless.

Layer 6 (Presentation) — Data formats, schema, encoding
- Standardize:
  - UTF-8 encoding
  - ISO 8601 timestamps (UTC preferred)
  - explicit decimal scale/precision rules
- Prefer columnar formats (Parquet/Delta) in lake for cost/performance.
- Schema evolution policy:
  - backward compatible changes default
  - breaking changes require new version/path/topic
- Validate schemas at ingestion; quarantine bad records (don’t pollute Silver/Gold).

Layer 7 (Application) — Data platform semantics (contracts, governance, quality)
- Data contracts per dataset/topic:
  - owner, purpose, SLA (freshness), schema, quality rules, retention
- Idempotent ingestion:
  - dedupe keys, watermarking, upserts where appropriate
- DLQ + replay:
  - for event-driven ingestion; never “drop silently”
- Observability:
  - freshness metrics, pipeline success rates, backlog, cost per run
- Access control:
  - least privilege, dataset-level permissions, RLS/CLS in serving layers when required

--------------------------------------------------------------------
3) Security & Governance Patterns (Microsoft-centric)
--------------------------------------------------------------------
Identity
- Entra ID everywhere; Managed Identity for ADF/Fabric/Functions where available.
- PIM for privileged roles; scoped RBAC per subscription/resource group.

Data access controls
- Storage: RBAC + ACLs (ADLS) carefully designed (least privilege).
- Serving: RLS (Power BI/SQL) for business-driven partitioning; CLS for sensitive columns.
- Keys/secrets: Key Vault (CMK when required); rotate certificates.

Governance
- Purview:
  - catalog + lineage for pipelines and lakehouse assets
  - classification labels for sensitive fields
- Azure Policy:
  - deny public endpoints, enforce private link, enforce tagging, restrict regions/SKUs

--------------------------------------------------------------------
4) Reliability & Data Freshness (Enterprise SLOs)
--------------------------------------------------------------------
Define SLOs (data platform specific)
- Freshness: e.g., “Gold tables updated within 15 minutes”
- Completeness: record counts within tolerance
- Accuracy: rule checks pass (null thresholds, referential integrity)
- Availability: serving endpoints and BI models

Operational patterns
- Watermarking:
  - track high-water mark per source/table; store in control table
- Idempotency:
  - reruns should not duplicate (merge/upsert patterns)
- Backpressure:
  - autoscale consumers on Event Hubs/queues; throttle upstream if downstream is hot
- Quarantine:
  - bad rows -> quarantine storage + defect metrics + triage workflow

--------------------------------------------------------------------
5) Data Modeling & Serving Best Practices
--------------------------------------------------------------------
- Gold is for consumption:
  - star schemas for BI, curated wide tables where justified
- Separate “product” datasets from “raw” datasets:
  - publish data products with contracts and documented semantics
- Semantic layer:
  - Power BI semantic models as governed consumption layer (certified datasets)

--------------------------------------------------------------------
6) Cost & Performance (Practical)
--------------------------------------------------------------------
- Partitioning strategy:
  - by date (and sometimes tenant/source) for lake
- File sizing:
  - avoid many tiny files; compact/optimize Silver/Gold regularly
- Avoid moving data unnecessarily:
  - pushdown filters, incremental loads, CDC where possible
- Choose the right integration tool:
  - ADF/Fabric pipelines for orchestration + movement
  - Spark for heavy transforms
  - SQL/Warehouse for serving + BI transforms

--------------------------------------------------------------------
7) “Default Controls” Checklist (copy/paste)
--------------------------------------------------------------------
Network
- Private endpoints + Private DNS Zones
- Deny public network access for data stores
- Central egress via Firewall/NAT

Identity
- Managed Identity everywhere possible
- Least privilege RBAC + PIM for admins

Data hygiene
- Bronze append-only; Silver validated; Gold curated
- Schema validation + quarantine
- Watermarks + idempotent reruns

Observability
- Pipeline run telemetry + alerts
- Freshness + backlog dashboards
- Cost per pipeline + anomaly alerts

Governance
- Purview catalog + lineage
- Data contracts + versioning policy
- Retention policies per zone (Bronze/Silver/Gold)

--------------------------------------------------------------------
If you describe your target (Fabric-first vs Azure-first, and whether you need 15-min freshness),
I can condense this into a single canonical blueprint with:
- minimal service set
- network posture (private-only vs edge-exposed)
- medallion implementation details
- SLOs + alert definitions
- a short “OSI troubleshooting playbook” for data issues
--------------------------------------------------------------------
