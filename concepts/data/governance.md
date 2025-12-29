# Data Governance — Best Practices

**Scope**: Data governance frameworks, operating models, metadata management, and policy-driven controls for enterprise data platforms.

**Purpose**: Use this when establishing data governance, designing operating models, implementing data catalogs, and managing data products. For security and privacy, see [Data Security](security.md). For operational practices, see [Data Operations](operations.md).

## Table of Contents

- [1. Data Governance Operating Model](#1-data-governance-operating-model)
- [2. Data Product Framework](#2-data-product-framework)
- [3. Metadata Management](#3-metadata-management)
- [4. Data Catalog](#4-data-catalog)
- [5. Data Lineage](#5-data-lineage)
- [6. Policy-Driven Controls](#6-policy-driven-controls)
- [7. Change Management](#7-change-management)
- [8. Documentation Standards](#8-documentation-standards)
- [9. Data Discovery](#9-data-discovery)
- [10. Compliance & Standards](#10-compliance--standards)

---

## 1. Data Governance Operating Model

### Federated Governance (Modern Default)

- **Central team**: Sets standards, controls, tooling (catalog, policy, identity)
- **Domain teams**: Own data products (meaning, SLAs, quality, access decisions)
- **Platform team**: Owns reliability, shared services, guardrails, landing zones

### Governance Roles

- **Data stewards**: Domain experts who understand data meaning and quality
- **Data owners**: Business owners responsible for data products
- **Data custodians**: Technical teams managing data infrastructure
- **Governance council**: Sets policies, resolves conflicts, approves standards

### Governance Principles

- **Decentralized execution, centralized standards**: Teams execute, central sets guardrails
- **Domain ownership**: Domain teams own their data products end-to-end
- **Self-service with guardrails**: Enable teams while enforcing standards
- **Data as product**: Treat datasets as products with contracts and SLAs

---

## 2. Data Product Framework

### Data Product Minimum Bar

A publishable dataset must have:

- **Owner + steward + support channel**: Clear ownership and support
- **Contract**: Schema + semantics + keys + SLAs + versioning policy
- **Quality checks + quality dashboard**: Automated quality monitoring
- **Lineage recorded**: Upstream sources and downstream consumers documented
- **Documentation + examples**: Getting started guides, sample queries
- **Access model**: Define roles, RLS/CLS/masking rules, approved use cases (implementation in [Data Security](security.md))

### Certification Levels

- **Draft**: Exploratory; no SLA; not for production use
- **Verified**: Quality checks exist; limited SLA; tested but not fully certified
- **Certified**: Governed, stable contract, SLOs, monitored, approved for BI/production

### Data Product Lifecycle

- **Discovery**: Identify need, assess feasibility
- **Development**: Build, test, document data product
- **Certification**: Quality checks, governance review, certification
- **Publication**: Publish to catalog, grant access
- **Evolution**: Version updates, deprecation planning
- **Retirement**: Deprecation, migration, archival

---

## 3. Metadata Management

### Metadata Categories

- **Technical metadata**: Schema, partitions, formats, owners, SLAs, retention, sensitivity tags
- **Business metadata**: Glossary terms, KPI definitions, calculation logic, "source of truth" flags
- **Operational metadata**: Pipeline owners, run frequency, failure modes, incident history
- **Social metadata**: User ratings, comments, usage statistics

### Metadata Standards

- **Naming conventions**: Consistent naming for datasets, columns, pipelines
- **Tagging strategy**: Domain tags, sensitivity labels, certification status
- **Classification**: Automatic classification based on content and usage
- **Ownership**: Clear ownership and stewardship assignment

### Metadata Quality

- **Completeness**: All required metadata fields populated
- **Accuracy**: Metadata reflects actual data and processes
- **Currency**: Metadata kept up-to-date
- **Consistency**: Consistent metadata across similar assets

---

## 4. Data Catalog

### Catalog Capabilities

- **Asset registration**: Register datasets, pipelines, reports, APIs
- **Metadata storage**: Centralized metadata repository
- **Search and discovery**: Full-text search, faceted search, recommendations
- **Access management**: Integration with access control systems
- **Usage tracking**: Track who uses what data, when, how

### Catalog Structure

- **Hierarchical organization**: Domains → data products → datasets
- **Taxonomy**: Standard taxonomy for organizing assets
- **Relationships**: Link related assets (datasets, pipelines, reports)
- **Versions**: Track versions of datasets and schemas

### Catalog Integration

- **Automated discovery**: Scan storage and databases to discover assets
- **Pipeline integration**: Extract metadata from pipeline runs
- **BI tool integration**: Catalog BI reports and semantic models
- **API integration**: Programmatic access to catalog

---

## 5. Data Lineage

### Lineage Types

- **Pipeline lineage**: Dataset A → transformation → dataset B
- **Column-level lineage**: Track column transformations (advanced, required for audits)
- **Data observability lineage**: Freshness and quality metrics tied to assets
- **Business lineage**: Business processes and reports using data

### Lineage Capture

- **Automated lineage**: Extract lineage from pipeline code and execution
- **Manual lineage**: Document lineage where automation isn't possible
- **Hybrid approach**: Combine automated and manual lineage
- **Lineage validation**: Verify lineage accuracy through testing

### Lineage Use Cases

- **Impact analysis**: Understand impact of schema changes
- **Root cause analysis**: Trace data quality issues to source
- **Compliance**: Demonstrate data provenance for audits
- **Documentation**: Visualize data flows for stakeholders

---

## 6. Policy-Driven Controls

### Policy Types

- **Access policies**: Define who can access what data based on sensitivity + domain + purpose (implementation in [Data Security](security.md))
- **Quality policies**: Require quality checks for certification (implementation in [Data Operations](operations.md))
- **Retention policies**: Define retention by data classification (implementation in [Data Operations](operations.md))
- **Tagging policies**: Require specific tags for data products
- **Naming policies**: Enforce naming conventions

### Automated Enforcement

- **Pre-deployment checks**: Validate policies before deployment
- **Runtime enforcement**: Enforce policies at runtime (access, masking)
- **Compliance monitoring**: Monitor policy compliance, alert on violations
- **Remediation**: Automated remediation where possible

### Policy Management

- **Policy as code**: Version control policies, test policy changes
- **Policy hierarchy**: Global policies, domain-specific overrides
- **Policy exceptions**: Process for requesting and approving exceptions
- **Policy review**: Regular review and update of policies

---

## 7. Change Management

### Data Contract Testing

- **Schema compatibility**: Check backward compatibility of schema changes
- **Sample payload validation**: Validate sample data against schema
- **Quality unit tests**: Test transform logic, business rules, calculations
- **Integration tests**: End-to-end validation of pipeline runs

### Data Testing Patterns

- **Unit tests**: Test individual transform functions and business rules
- **Integration tests**: Test complete pipeline runs with test data
- **Data validation tests**: Referential integrity, constraints, expected ranges
- **Regression tests**: Ensure changes don't break existing functionality

### Deprecation Lifecycle

- **Announce**: Notify consumers of upcoming deprecation
- **Dual-run**: Run old and new versions in parallel
- **Cutover**: Migrate consumers to new version
- **Retire**: Remove deprecated version after migration period

### Versioning Strategy

- **Semantic versioning**: Major.minor.patch for breaking/non-breaking changes
- **Backward compatibility**: Maintain backward compatibility when possible
- **Migration guides**: Provide clear migration paths for breaking changes
- **Version documentation**: Document changes in each version

---

## 8. Documentation Standards

### Dataset Documentation

- **Description**: What the dataset contains, purpose, use cases
- **Schema documentation**: Column descriptions, data types, examples
- **Sample queries**: Common query patterns, joins, pitfalls
- **Business context**: Business meaning, calculation logic, KPI definitions
- **Changelog**: Schema changes, deprecations, breaking changes with dates

### Getting Started Guides

- **Quick start**: How to access and use the dataset
- **Sample queries**: Common query patterns
- **Joins**: How to join with other tables
- **Pitfalls**: Common mistakes, edge cases
- **Examples**: Real-world use cases

### Documentation Maintenance

- **Keep current**: Update documentation when data changes
- **Version control**: Track documentation changes
- **Review process**: Regular review of documentation accuracy
- **User feedback**: Incorporate user feedback into documentation

---

## 9. Data Discovery

### Search Capabilities

- **Full-text search**: Search across metadata, schemas, documentation, tags
- **Faceted search**: Filter by domain, sensitivity, certification status
- **Semantic search**: Understand intent, not just keywords
- **Advanced filters**: Filter by date, owner, usage, quality metrics

### Browsing Patterns

- **Hierarchical navigation**: Browse by domain, data product, source system
- **Related assets**: "Similar datasets", "frequently used together"
- **Trending**: Most accessed datasets, recently added
- **Recommendations**: Personalized recommendations based on usage

### Discovery Features

- **Tags & classifications**: Domain tags, sensitivity labels, certification status
- **Ratings and reviews**: User ratings and comments on datasets
- **Usage statistics**: How often datasets are accessed, by whom
- **Data preview**: Preview data before accessing

---

## 10. Compliance & Standards

### Ontario-Specific Requirements

- **GO-ITS standards**: Adhere to Government of Ontario IT Standards for data management, security, and interoperability
- **Open Data Directive**: Government data open by default unless exempted (privacy, security, confidentiality, commercial sensitivity)
- **Anti-Racism Act**: Race-based data collection requirements for public sector organizations (identify and monitor racial disparities)

### Data Standards

- **Data formats**: Standard formats (Parquet, Delta, JSON)
- **Naming conventions**: Consistent naming across organization
- **Schema standards**: Standard schemas for common data types
- **Quality standards**: Minimum quality thresholds

### Compliance Practices

- **Data mapping**: Map data flows and identify compliance requirements (for governance standards)
- **Audit trails**: Maintain audit trails for governance compliance (security audit trails in [Data Security](security.md))
- **Documentation**: Document compliance measures
- **Regular audits**: Internal and external compliance audits (governance and data standards)

---

> **Note**: For security and privacy controls, see [Data Security](security.md). For operational practices (quality, integrity, reliability), see [Data Operations](operations.md).

