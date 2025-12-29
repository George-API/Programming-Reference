# Data Security & Privacy — Best Practices

**Scope**: Data security and privacy controls, patterns, and best practices for protecting data assets.

**Purpose**: Use this when designing data security controls, implementing privacy protections, and establishing access management. For governance practices, see [Data Governance](governance.md). For operational practices, see [Data Operations](operations.md).

## Table of Contents

- [1. Threat Modeling & Risk Assessment](#1-threat-modeling--risk-assessment)
- [2. Access Control Patterns](#2-access-control-patterns)
- [3. Sensitive Data Protection](#3-sensitive-data-protection)
- [4. Encryption & Key Management](#4-encryption--key-management)
- [5. Privacy by Design](#5-privacy-by-design)
- [6. De-identification & Anonymization](#6-de-identification--anonymization)
- [7. Auditing & Monitoring](#7-auditing--monitoring)
- [8. Compliance & Regulatory Requirements](#8-compliance--regulatory-requirements)
- [9. Data Loss Prevention](#9-data-loss-prevention)
- [10. Incident Response](#10-incident-response)

---

## 1. Threat Modeling & Risk Assessment

### Threat Model Mindset

- **Who can exfiltrate?**: Insiders, compromised identities, misconfigurations, external attackers
- **What is the blast radius?**: Tenant/domain isolation, network segmentation, data classification
- **Attack vectors**: Unauthorized access, data exfiltration, privilege escalation, insider threats

### Risk Assessment

- **Data classification**: Classify data by sensitivity (public, internal, confidential, restricted)
- **Risk scoring**: Assess likelihood and impact of threats
- **Vulnerability assessment**: Regular security assessments, penetration testing
- **Threat intelligence**: Monitor emerging threats and adapt controls

---

## 2. Access Control Patterns

### Authentication

- **Multi-factor authentication (MFA)**: Require MFA for sensitive data access
- **Single sign-on (SSO)**: Centralized authentication, reduce credential sprawl
- **Managed identities**: Use service principals/managed identities for service-to-service access
- **Certificate-based auth**: For high-security scenarios

### Authorization Models

- **Role-based access control (RBAC)**: Assign permissions based on roles
- **Attribute-based access control (ABAC)**: Policy based on attributes (domain, sensitivity, geography, time)
- **Row-level security (RLS)**: Filter data at row level based on user attributes
- **Column-level security (CLS)**: Mask or restrict access to sensitive columns

### Access Patterns

- **Principle of least privilege**: Grant minimum necessary permissions
- **Just-in-time (JIT) access**: Time-bound, approved access for elevated permissions
- **Break-glass accounts**: Tightly controlled, monitored, time-bound, reviewed emergency access
- **Privileged access management (PAM)**: Manage and monitor privileged accounts

### Access Lifecycle

- **Onboarding**: Provision access based on role and need
- **Periodic recertification**: Re-approve dataset access on schedule (quarterly/annually)
- **Offboarding**: Revoke access immediately upon role change or departure
- **Access reviews**: Regular audits of who has access to what

---

## 3. Sensitive Data Protection

### Data Classification

- **Classification levels**: Public, Internal, Confidential, Restricted
- **Sensitivity tags**: Automatically tag data based on content (PII, PHI, financial)
- **Classification automation**: Use ML/AI to detect sensitive data patterns
- **Metadata-driven**: Store classification in metadata catalog

### Safe Views Pattern

- **Per-domain safe views**: Curated, masked extracts as default for analysts
- **Tiered access**: Raw data → masked → aggregated → synthetic (based on need)
- **View-based access**: Grant access to views, not underlying tables
- **Dynamic masking**: Mask sensitive data at query time

### Data Masking Techniques

- **Static masking**: Replace sensitive data with realistic but fake data
- **Dynamic masking**: Mask data at query time based on user permissions
- **Format-preserving encryption**: Encrypt while maintaining format
- **Tokenization**: Replace sensitive data with tokens

---

## 4. Encryption & Key Management

### Encryption at Rest

- **Storage encryption**: Encrypt data at storage layer (transparent encryption)
- **Application-level encryption**: Encrypt sensitive fields before storage
- **Database encryption**: Encrypt entire databases or specific columns
- **File-level encryption**: Encrypt files before storing in object storage

### Encryption in Transit

- **TLS/SSL**: Encrypt all data in transit (minimum TLS 1.2, prefer 1.3)
- **VPN/Private links**: Use private network connections for sensitive data
- **Certificate management**: Proper certificate lifecycle management

### Key Management

- **Centralized key management**: Use key vaults (Azure Key Vault, AWS KMS, HashiCorp Vault)
- **Key rotation**: Regular key rotation policies and procedures
- **Key separation**: Separate keys for different environments/data types
- **Double-key encryption**: Org key + domain key for higher assurance (advanced)
- **Hardware security modules (HSM)**: For highest security requirements

### Key Lifecycle

- **Key generation**: Cryptographically secure key generation
- **Key storage**: Secure storage, never in code or config files
- **Key access**: Limit access to keys, audit key usage
- **Key rotation**: Automated rotation, support for multiple key versions during transition
- **Key revocation**: Ability to revoke compromised keys

---

## 5. Privacy by Design

### Core Principles

- **Proactive not reactive**: Build privacy into design, not as afterthought
- **Privacy as default**: Default to most privacy-protective settings
- **Full functionality**: Privacy and functionality together, not trade-offs
- **End-to-end security**: Security throughout data lifecycle
- **Visibility and transparency**: Clear privacy policies and practices

### Separation of Duties

- **Limited PII access**: Only specific roles can access raw identifiers (PII)
- **Role segregation**: Separate roles for data collection, processing, and access
- **Approval workflows**: Require approvals for sensitive data access
- **Dual control**: Require multiple approvals for high-risk operations

### Consent & Purpose Tracking

- **Consent management**: Track and manage user consent for data collection/use
- **Purpose limitation**: Collect and use data only for stated purposes
- **Purpose metadata**: Attach "allowed usage" metadata to datasets
- **Policy enforcement**: Enforce usage policies where possible (technical controls)
- **Consent withdrawal**: Support for users to withdraw consent

### Right-to-Deletion Workflows

- **Data location**: Locate identifiers across lake/warehouse/serving layers
- **Deletion propagation**: Propagate deletion/soft-delete markers across systems
- **Audit proof**: Maintain audit trail of deletions for compliance
- **Soft delete**: Use soft deletes where hard deletes aren't feasible
- **Backup handling**: Address data in backups and archives

---

## 6. De-identification & Anonymization

### De-identification Ladder

- **Masking**: Replace sensitive values with masks (e.g., `***-***-1234`)
- **Pseudonymization**: Replace identifiers with pseudonyms (reversible with key)
- **Tokenization**: Replace with tokens (maintain referential integrity)
- **Aggregation**: Aggregate data to remove individual identifiers
- **Synthetic data**: Generate synthetic data that preserves statistical properties

### Techniques

- **k-anonymity**: Each record indistinguishable from k-1 others
- **l-diversity**: k-anonymity + diversity in sensitive attributes
- **t-closeness**: Distribution of sensitive attributes close to population
- **Differential privacy**: Add calibrated noise to protect individual privacy

### Re-identification Risk

- **Risk assessment**: Assess risk of re-identification
- **Context matters**: Consider data context and available external data
- **Regular review**: Re-assess risk as new data becomes available
- **Documentation**: Document de-identification methods and residual risks

---

## 7. Auditing & Monitoring

### Audit Logging

- **Comprehensive logging**: Log all data access, modifications, and administrative actions
- **Immutable logs**: Append-only audit log sinks (cannot be modified)
- **Structured logs**: Machine-readable format for analysis
- **Retention**: Retain audit logs per regulatory requirements (often 7+ years)

### What to Audit

- **Access events**: Who accessed what data, when, from where
- **Data modifications**: Changes to data, schema, configurations
- **Privilege changes**: Grant/revoke of permissions, role changes
- **Failed access attempts**: Authentication failures, authorization denials
- **Data exports**: Downloads, exports, API calls that retrieve data

### Monitoring & Alerting

- **Anomaly detection**: ML-based detection of unusual patterns
- **Alert patterns**:
  - Unusual volume (large data access)
  - Unusual time (access outside normal hours)
  - Unusual geography (access from unexpected locations)
  - Privilege escalation (sudden permission increases)
  - Failed access spikes (potential attack)
- **Real-time alerts**: Immediate alerts for high-risk events
- **Dashboards**: Security dashboards for visibility

### Access Recertification

- **Periodic reviews**: Regular access reviews (quarterly/annually)
- **Automated workflows**: Automated recertification workflows
- **Owner approval**: Dataset owners approve continued access
- **Remediation**: Automated revocation of unapproved access

---

## 8. Compliance & Regulatory Requirements

### Ontario-Specific Requirements

- **FIPPA (Freedom of Information and Protection of Privacy Act)**: Governs collection, use, and disclosure of personal information by public institutions
- **PHIPA (Personal Health Information Protection Act)**: Applies to personal health information (health information custodians)
- **GO-ITS (Government of Ontario IT Standards)**: IT security standards for public sector systems
- **Open Data Directive**: Government data open by default unless exempted
- **Anti-Racism Act**: Race-based data collection requirements

### General Compliance Frameworks

- **GDPR**: General Data Protection Regulation (EU)
- **CCPA/CPRA**: California Consumer Privacy Act
- **HIPAA**: Health Insurance Portability and Accountability Act (US)
- **SOC 2**: Security and availability controls
- **ISO 27001**: Information security management

### Compliance Practices

- **Data mapping**: Map data flows and identify security/privacy compliance requirements
- **Privacy impact assessments**: Assess privacy impact of new projects
- **Documentation**: Document security controls and compliance measures
- **Regular audits**: Internal and external security audits
- **Incident reporting**: Report breaches per regulatory requirements

---

## 9. Data Loss Prevention (DLP)

### DLP Policies

- **Content inspection**: Scan data for sensitive patterns (SSN, credit cards, etc.)
- **Policy enforcement**: Block or warn on policy violations
- **Export controls**: Monitor and control data exports/downloads
- **Email DLP**: Scan emails for sensitive data
- **Endpoint DLP**: Monitor data on endpoints (laptops, USB drives)

### DLP Patterns

- **Prevent exfiltration**: Block unauthorized data transfers
- **Monitor access**: Log and alert on suspicious access patterns
- **Watermarking**: Embed watermarks to track data origin
- **Data tagging**: Tag data with sensitivity labels for DLP enforcement

---

## 10. Incident Response

### Incident Types (Security)

- **Data breach**: Unauthorized access to sensitive data
- **Privilege escalation**: Unauthorized privilege gain
- **Data exfiltration**: Unauthorized data export/transfer
- **Insider threat**: Malicious or negligent insider actions
- **Configuration errors**: Misconfigurations exposing data
- **Note**: Operational incidents (freshness, quality, performance) covered in [Data Operations](operations.md)

### Response Procedures

- **Detection**: Identify and classify security incidents
- **Containment**: Isolate affected systems, revoke access
- **Investigation**: Root cause analysis, impact assessment
- **Remediation**: Fix vulnerabilities, restore systems
- **Communication**: Notify stakeholders, regulatory bodies if required
- **Post-incident**: Lessons learned, process improvements

### Prevention

- **Security training**: Regular security awareness training
- **Phishing simulation**: Test and train on phishing attacks
- **Vulnerability management**: Regular patching and updates
- **Security reviews**: Code reviews, architecture reviews
- **Penetration testing**: Regular security testing

---

> **Note**: For governance practices (operating model, catalog, policies), see [Data Governance](governance.md). For operational practices (quality, integrity, reliability), see [Data Operations](operations.md).

