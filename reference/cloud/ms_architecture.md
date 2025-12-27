# Microsoft Data Platform Implementation

**Scope**: Azure/Microsoft Fabric-centric enterprise data platform implementation (ingest → lake/lakehouse → serving → BI).

## Table of Contents

- [Core Stack](#core-stack)
- [1. Reference Architecture (Medallion Lakehouse)](#1-reference-architecture-medallion-lakehouse)
- [2. Microsoft Fabric Patterns](#2-microsoft-fabric-patterns)
- [3. Security & Governance (Microsoft Data Platform)](#3-security--governance-microsoft-data-platform)
- [4. Reliability & Data Freshness (Implementation)](#4-reliability--data-freshness-implementation)
- [5. Data Modeling & Serving](#5-data-modeling--serving)
- [6. Cost & Performance](#6-cost--performance)
- [7. Default Controls Checklist](#7-default-controls-checklist)

---

## Core Stack

**Typical stack:**

- **Ingestion/Orchestration**: Azure Data Factory (ADF) / Fabric Data Pipelines
- **Streaming**: Event Hubs (telemetry/streams), Event Grid (events), Service Bus (commands/queues)
- **Storage/Lake**: ADLS Gen2 (OneLake in Fabric context), Delta/Parquet
- **Processing**: Fabric (Spark/Lakehouse) / Synapse / Databricks (org-dependent)
- **Serving**: Azure SQL / Fabric Warehouse / Synapse SQL
- **Semantic/BI**: Power BI
- **Governance**: Microsoft Purview
- **Security/Monitoring**: Entra ID, Key Vault, Defender for Cloud, Azure Monitor/App Insights

---

## 1. Reference Architecture (Medallion Lakehouse)

### A) Sources

- SaaS (Dynamics, ServiceNow, etc.), on-prem DBs, files, APIs, IoT/telemetry

### B) Ingestion Layer

- **Batch**: ADF / Fabric Pipelines (copy + transforms) → Bronze
- **CDC**: source CDC → landing → Bronze (or event stream)
- **Streaming**: Event Hubs → stream processing → Bronze/Silver

### C) Lakehouse Layers (Medallion)

- **Bronze**: raw/append-only, immutable, source-aligned partitions
- **Silver**: cleaned/standardized, deduped, schema-enforced, conformed dimensions
- **Gold**: curated business entities / aggregates for analytics + BI serving

### D) Serving

- **Warehouse/SQL**: Fabric Warehouse / Synapse / Azure SQL for BI-friendly consumption
- **APIs**: lightweight serving via App Service/Functions if operational consumers need data products

### E) Governance/Operations

- Purview for catalog + lineage + classification
- Monitor + Log Analytics for pipeline/run telemetry, freshness, failures
- Data quality gates + quarantine + replay tooling

---

## 2. Microsoft Fabric Patterns

### OneLake (Unified Storage)

- Single logical lake across all workspaces; no data movement between workspaces
- **Shortcuts**: reference external data (ADLS, S3, Google Cloud) without copying
- **Delta format**: default for lakehouse tables; ACID transactions, time travel
- **Partitioning**: by date/tenant/source; optimize file sizes (128MB-1GB target)

### Medallion Architecture Implementation

- **Bronze (Raw)**: 
  - OneLake location: `lakehouse/bronze/{source}/{table}/`
  - Append-only; immutable; source-aligned partitions
  - Schema-on-read; minimal validation
- **Silver (Cleaned)**:
  - OneLake location: `lakehouse/silver/{domain}/{table}/`
  - Deduplicated; standardized; schema-enforced
  - Quality checks; quarantine bad records
- **Gold (Curated)**:
  - OneLake location: `lakehouse/gold/{domain}/{table}/`
  - Business entities; star schemas; aggregates
  - Certified datasets; documented semantics

### Semantic Models (Power BI)

- **Import mode**: data copied into Power BI (fast, limited size)
- **DirectQuery**: live queries to Fabric Warehouse (real-time, larger datasets)
- **Composite models**: mix import + DirectQuery for optimal performance
- **Certified datasets**: governance-approved; promoted in Power BI service

### Data Networking (Data Platform Specific)

- **ADLS Private Link**: Private endpoints for OneLake/ADLS Gen2 access
- **Fabric Private Link**: Private endpoints for Fabric workspace access (preview)
- **Data plane isolation**: separate subnets for data ingestion vs serving
- **Egress control**: NAT Gateway for stable outbound IPs (source systems allowlists)

> **Note**: For general networking patterns, see [Architecture](cloud_architecture.md). For OSI troubleshooting, see [OSI Troubleshooting Reference](cloud_osi.md).

---

## 3. Security & Governance (Microsoft Data Platform)

### Identity

- Entra ID everywhere; Managed Identity for ADF/Fabric/Functions where available.
- PIM for privileged roles; scoped RBAC per subscription/resource group.

### Data access controls

- **Storage**: RBAC + ACLs (ADLS) carefully designed (least privilege).
- **Serving**: RLS (Power BI/SQL) for business-driven partitioning; CLS for sensitive columns.
- **Keys/secrets**: Key Vault (CMK when required); rotate certificates.

### Governance

- **Purview**:
  - catalog + lineage for pipelines and lakehouse assets
  - classification labels for sensitive fields
  - data contracts enforcement (schema validation)
- **Azure Policy**:
  - deny public endpoints on data stores, enforce private link, enforce tagging, restrict regions/SKUs
- **Fabric governance**:
  - workspace-level access control, item-level permissions
  - data sensitivity labels, retention policies

> **Note**: For data management concepts (operating model, quality, integrity), see [Data Management Best Practices](data_management.md).

---

## 4. Reliability & Data Freshness (Implementation)

> **Note**: For data SLO concepts (what they are, why they matter), see [Data Management Best Practices - Reliability](data_management.md#8-reliability-resilience-and-data-sre-advanced).

### Implementing Data SLOs (Azure/Fabric)

- **Freshness SLO**: Monitor pipeline completion times; alert on SLA breaches
  - Fabric: Use pipeline run metrics + Log Analytics queries
  - Alert when Gold partition update exceeds SLA window
- **Completeness SLO**: Compare record counts vs source control totals
  - Fabric: Use data quality checks in pipelines; publish metrics
- **Accuracy SLO**: Quality check pass rates
  - Fabric: Integrate Great Expectations or custom checks; publish to dashboard
- **Availability SLO**: Power BI semantic model uptime; Warehouse query success rates

### Operational patterns

- **Watermarking**:
  - track high-water mark per source/table; store in control table
- **Idempotency**:
  - reruns should not duplicate (merge/upsert patterns)
- **Backpressure**:
  - autoscale consumers on Event Hubs/queues; throttle upstream if downstream is hot
- **Quarantine**:
  - bad rows → quarantine storage + defect metrics + triage workflow

---

## 5. Data Modeling & Serving

### Gold Layer Design

- **Star schemas**: fact tables + dimension tables for BI (Power BI optimized)
- **Curated wide tables**: denormalized where justified (fewer joins, faster queries)
- **Partitioning**: by date for time-series; by tenant for multi-tenant
- **Indexing**: Fabric Warehouse automatic indexing; manual optimization for large tables

### Data Products

- **Fabric data products**: publish Gold tables as certified datasets
- **Contracts**: schema, semantics, keys, SLAs defined in Purview
- **Documentation**: Power BI semantic model descriptions, sample queries, changelog

> **Note**: For data contract concepts and operating model, see [Data Management Best Practices](data_management.md).

---

## 6. Cost & Performance (Practical)

- **Partitioning strategy**:
  - by date (and sometimes tenant/source) for lake
- **File sizing**:
  - avoid many tiny files; compact/optimize Silver/Gold regularly
- **Avoid moving data unnecessarily**:
  - pushdown filters, incremental loads, CDC where possible
- **Choose the right integration tool**:
  - ADF/Fabric pipelines for orchestration + movement
  - Spark for heavy transforms
  - SQL/Warehouse for serving + BI transforms

---

## 7. "Default Controls" Checklist

### Network

- Private endpoints + Private DNS Zones
- Deny public network access for data stores
- Central egress via Firewall/NAT

### Identity

- Managed Identity everywhere possible
- Least privilege RBAC + PIM for admins

### Data hygiene

- Bronze append-only; Silver validated; Gold curated
- Schema validation + quarantine
- Watermarks + idempotent reruns

### Observability

- Pipeline run telemetry + alerts
- Freshness + backlog dashboards
- Cost per pipeline + anomaly alerts

### Governance

- Purview catalog + lineage
- Data contracts + versioning policy (see [Data Management Best Practices](data_management.md))
- Retention policies per zone (Bronze/Silver/Gold)

