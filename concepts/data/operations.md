# Data Operations — Best Practices

**Scope**: Data quality, integrity, lifecycle management, and reliability practices for operational data platforms.

**Purpose**: Use this when implementing data quality systems, ensuring data integrity, managing data lifecycle, and establishing reliability practices. For governance practices, see [Data Governance](governance.md). For security and privacy, see [Data Security](security.md).

## Table of Contents

- [1. Data Quality Systems](#1-data-quality-systems)
- [2. Quality Gates](#2-quality-gates)
- [3. Quality Rules & Validation](#3-quality-rules--validation)
- [4. Data Integrity Patterns](#4-data-integrity-patterns)
- [5. Temporal Integrity](#5-temporal-integrity)
- [6. Data Lifecycle Management](#6-data-lifecycle-management)
- [7. Retention & Archival](#7-retention--archival)
- [8. Reliability & Data SRE](#8-reliability--data-sre)
- [9. Observability & Monitoring](#9-observability--monitoring)
- [10. Incident Management](#10-incident-management)

---

## 1. Data Quality Systems

### Quality Dimensions

- **Completeness**: Required fields populated, no missing values
- **Accuracy**: Data correctly represents real-world entities
- **Consistency**: Data consistent across systems and time
- **Validity**: Data conforms to defined rules and formats
- **Timeliness**: Data available when needed, meets freshness SLAs
- **Uniqueness**: No duplicate records

### Quality Metrics

- **Pass rate**: Percentage of records passing quality checks
- **Defect rate**: Percentage of records with quality issues
- **Coverage**: Percentage of data covered by quality checks
- **Drift metrics**: Measure distribution shifts over time

### Quality Monitoring

- **Real-time monitoring**: Monitor quality during pipeline execution
- **Quality dashboards**: Visualize quality metrics and trends
- **Alerting**: Alert on quality degradation or threshold breaches
- **Quality reports**: Regular quality reports for stakeholders

---

## 2. Quality Gates

### Gate Placement

- **Ingestion gate (Bronze)**: Schema/format validation + source availability checks
- **Transformation gate (Silver)**: Standardization + dedupe + domain rules
- **Serving gate (Gold)**: KPI sanity + reconciliations + referential checks

### Gate Enforcement

- **Block on failure**: Stop pipeline if critical checks fail
- **Warn on failure**: Continue pipeline but alert on failures
- **Quarantine**: Route bad records to quarantine for review
- **Auto-remediation**: Automatically fix common issues where safe

### Gate Configuration

- **Severity levels**: Critical, warning, informational
- **Thresholds**: Define acceptable quality thresholds
- **Exception handling**: Process for handling exceptions
- **Gate bypass**: Controlled process for bypassing gates when necessary

---

## 3. Quality Rules & Validation

### Rule Categories

- **Structural**: Schema, types, required columns, nullability
- **Content**: Ranges, enums, regex formats, cross-field rules
- **Relationship**: Uniqueness, referential integrity, join coverage
- **Statistical**: Drift detection (distribution shifts, sudden spikes/drops)
- **Timeliness**: Freshness SLA compliance, late-arrival rates
- **Business rules**: Domain-specific validation rules

### Validation Patterns

- **Schema validation**: Validate structure and types
- **Range validation**: Check values within expected ranges
- **Format validation**: Validate formats (email, phone, date)
- **Referential validation**: Check foreign key relationships
- **Custom validation**: Business-specific validation logic

### Quarantine & Defect Lifecycle

- **Quarantine storage**: Separate storage for bad records
- **Reason codes**: Categorize defects with reason codes
- **Defect backlog**: Owned by domain steward, SLA for remediation
- **Replay/reprocess**: Tooling for corrected data reprocessing
- **Defect analysis**: Analyze patterns to prevent future defects

---

## 4. Data Integrity Patterns

### Keys and Identity

- **Canonical business keys**: Defined in contract (natural keys where stable)
- **Surrogate keys**: Deterministic hash keys to stabilize across reruns/backfills
- **Composite keys**: Multi-column keys for complex relationships
- **Key stability**: Ensure keys don't change unexpectedly

### Deduplication

- **Survivorship rules**: Explicit rules for handling duplicates
  - Latest timestamp wins
  - Highest priority source wins
  - Non-null preference
  - Most complete record wins
- **Deduplication strategy**: When and how to deduplicate
- **Deduplication keys**: Identify duplicates using key fields

### Referential Integrity

- **Foreign key constraints**: Enforce relationships between tables
- **Orphan detection**: Identify records with missing parent records
- **Cascade rules**: Define behavior on parent record changes
- **Integrity checks**: Regular checks for referential integrity violations

### Immutability & Auditability

- **Bronze append-only**: Never edit raw data, store corrections as new records
- **Version history**: Track changes to data over time
- **Data change tracking**: Track data modifications for operational purposes (security audit trails in [Data Security](security.md))
- **Reproducible transforms**: Version code + config + dependencies for every run

---

## 5. Temporal Integrity

### Late Arriving Data

- **Watermark strategy**: Track high-water mark per source/table
- **Allowed lateness window**: Define acceptable delay for late arrivals
- **Backfill procedures**: Process late-arriving data correctly
- **Time-based partitioning**: Partition by time for efficient processing

### Slowly Changing Dimensions (SCD)

- **Type 1 (Overwrite)**: No history, update in place
- **Type 2 (Full history)**: New row for each change, track effective dates
- **Type 3 (Limited history)**: Previous value column, limited history
- **Hybrid approaches**: Combine SCD types based on business needs

### Effective-Dated Joins

- **Point-in-time accuracy**: Ensure facts map to correct dimension version
- **As-of joins**: Join facts to dimensions as of specific date
- **Temporal queries**: Query data as it existed at specific points in time
- **Version tracking**: Track dimension versions for accurate joins

### Time Zones & Calendars

- **UTC storage**: Store timestamps in UTC, convert for display
- **Time zone handling**: Proper time zone conversion
- **Business calendars**: Support for business days, fiscal calendars
- **Daylight saving time**: Handle DST transitions correctly

---

## 6. Data Lifecycle Management

### Data Zones & Retention

- **Bronze (Raw)**: Shorter retention if sourced elsewhere + storage cost constraints
- **Silver (Cleaned)**: Moderate retention (replay/backfill window)
- **Gold (Curated)**: Retention aligned to reporting/regulatory requirements
- **Zone-specific policies**: Different retention policies per zone

### Data Classification for Lifecycle

- **Hot data**: Frequently accessed, keep in fast storage
- **Warm data**: Occasionally accessed, standard storage
- **Cold data**: Rarely accessed, archive to cheaper storage
- **Automated tiering**: Automatically move data between tiers

### Data Archival

- **Archive criteria**: When to archive data (age, access patterns)
- **Archive format**: Compressed, immutable format for archives
- **Archive location**: Cost-effective storage for archives
- **Restore procedures**: Tested procedures for restoring archived data

---

## 7. Retention & Archival

### Retention Policies

- **Policy-driven**: Define retention policies based on data classification
- **Regulatory retention**: Retain data per regulatory requirements (FIPPA, PHIPA)
- **Business retention**: Retain data per business needs
- **Retention schedules**: Approved retention schedules for records

### Deletion Patterns

- **Hard delete**: Permanently remove data
- **Soft delete**: Mark as deleted, retain for recovery
- **Crypto-shredding**: Destroy encryption keys to make data unreadable
- **Legal hold**: Override deletion for legal/compliance holds

### Deletion Workflows

- **Automated deletion**: Automatically delete data per retention policies
- **Approval workflows**: Require approval for data deletion
- **Deletion tracking**: Track data deletions for operational purposes (security audit trails in [Data Security](security.md))
- **Exception handling**: Process for legal holds and exceptions

### Ontario-Specific Requirements

- **FIPPA retention**: Retain personal information only as long as necessary
- **Disposal requirements**: Securely dispose of personal information when no longer needed
- **Record retention schedules**: Follow approved retention schedules for government records

---

## 8. Reliability & Data SRE

### Data SLOs

- **Freshness SLO**: Percentage of partitions updated within SLA window
- **Completeness SLO**: Percentage of expected records received
- **Quality SLO**: Percentage of critical checks passing
- **Availability SLO**: Query/serving uptime for consumers
- **Latency SLO**: Query response time targets

### Error Budgets

- **Error budget concept**: Allowable failures before SLO breach
- **Budget tracking**: Monitor error budget consumption
- **Budget-based alerting**: Alert when error budget at risk
- **Budget allocation**: Allocate budget across different failure types

### Reliability Patterns

- **Idempotency**: Ensure pipelines can be safely rerun
- **Exactly-once semantics**: Achieve exactly-once processing where needed
- **At-least-once + deduplication**: Accept at-least-once, deduplicate
- **Backpressure handling**: Handle downstream capacity issues

---

## 9. Observability & Monitoring

### Monitoring Metrics

- **Pipeline metrics**: Run status, duration, success/failure rates
- **Data metrics**: Record counts, data volume, freshness
- **Quality metrics**: Pass rates, defect rates, drift metrics
- **Performance metrics**: Query latency, throughput, resource usage
- **Cost metrics**: Cost per dataset, pipeline, tenant

### Alerting Strategy

- **SLO-based alerts**: Alert on SLO breaches
- **Anomaly detection**: Alert on unusual patterns
- **Threshold alerts**: Alert when metrics exceed thresholds
- **Alert fatigue**: Avoid over-alerting, focus on actionable alerts

### Dashboards

- **Operational dashboards**: Real-time pipeline and data status
- **Quality dashboards**: Data quality metrics and trends
- **Cost dashboards**: Cost visibility and optimization opportunities
- **SLO dashboards**: SLO compliance and error budget status

### Logging

- **Structured logging**: Machine-readable log format
- **Log aggregation**: Centralized log collection and analysis
- **Log retention**: Retain logs per requirements
- **Log analysis**: Analyze logs for troubleshooting and insights

---

## 10. Incident Management

### Incident Types (Operational)

- **Freshness breach**: Pipeline fails, data not updated on time
- **Schema break**: Source schema changes, breaks pipeline
- **Quality degradation**: Quality checks failing, data issues
- **Performance issues**: Slow queries, pipeline delays
- **Data loss**: Data corruption or loss (security incidents in [Data Security](security.md))

### Incident Response

- **Detection**: Identify and classify incidents
- **Triage**: Assess severity and impact
- **Containment**: Isolate issue, prevent spread
- **Resolution**: Fix root cause, restore service
- **Communication**: Notify stakeholders, provide updates
- **Post-incident**: Lessons learned, process improvements

### Incident Playbooks

- **Freshness breach**: Identify dependency, rerun safe path, communicate ETA, prevent recurrence
- **Schema break**: Block pipeline, quarantine, negotiate contract change, version bump
- **DLQ spike**: Classify transient/permanent, replay, add idempotency fix if needed
- **Quality breach**: Investigate root cause, fix data, update quality rules

### Backpressure & Scaling

- **Stream lag alerts**: Consumer lag thresholds + autoscale rules
- **Queue depth alerting**: Scale consumers; throttle producers if necessary
- **Concurrency limits**: Prevent DB connection storms and SNAT exhaustion
- **Resource scaling**: Auto-scale based on load

### Replay & Backfill

- **Replay procedures**: Procedures for replaying data
- **Backfill tooling**: Tools for backfilling historical data
- **Incremental backfill**: Backfill only missing partitions
- **Full backfill**: Complete data reprocessing when needed
- **Backfill testing**: Test backfill procedures regularly

---

> **Note**: For governance practices (operating model, catalog, policies), see [Data Governance](governance.md). For security and privacy controls, see [Data Security](security.md).

