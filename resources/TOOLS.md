# Enterprise Tools Reference

**Purpose**: Key tools for Microsoft-centric IT project management and development environments.

**Related**: [References](References.md) | [Terminology](../TERMINOLOGY.md)

---

## Table of Contents

- [IDE & Editors](#ide--editors)
- [Database Tools](#database-tools)
- [CLI Tools](#cli-tools)
- [API & Testing Tools](#api--testing-tools)
- [Data Quality & Observability](#data-quality--observability)
- [Data Catalog & Governance](#data-catalog--governance)
- [Container Tools](#container-tools)
- [Code Security & Scanning](#code-security--scanning)
- [Diagramming](#diagramming)
- [Work Tracking](#work-tracking)
- [Documentation & Collaboration](#documentation--collaboration)

---

## IDE & Editors

| Tool | Focus | Link |
|------|-------|------|
| **Visual Studio** | Full IDE (.NET, C++, Python) | [visualstudio.microsoft.com](https://visualstudio.microsoft.com/) |
| **Visual Studio Code** | Lightweight editor | [code.visualstudio.com](https://code.visualstudio.com/) |
| **JetBrains Rider** | .NET IDE | [jetbrains.com/rider](https://www.jetbrains.com/rider/) |
| **JetBrains DataGrip** | Database IDE | [jetbrains.com/datagrip](https://www.jetbrains.com/datagrip/) |

**Key VS Code Extensions**: Azure Repos, Bicep, Docker, GitLens, Pylance, Remote-SSH, Thunder Client

---

## Database Tools

| Tool | Focus | Link |
|------|-------|------|
| **SQL Server Management Studio** | SQL Server administration | [learn.microsoft.com/ssms](https://learn.microsoft.com/sql/ssms/) |
| **Azure Data Studio** | Cross-platform SQL tool | [learn.microsoft.com/azure-data-studio](https://learn.microsoft.com/azure-data-studio/) |
| **DBeaver** | Universal database tool | [dbeaver.io](https://dbeaver.io/) |
| **pgAdmin** | PostgreSQL administration | [pgadmin.org](https://www.pgadmin.org/) |

---

## CLI Tools

| Tool | Focus | Link |
|------|-------|------|
| **Azure CLI** | Azure resource management | [learn.microsoft.com/cli/azure](https://learn.microsoft.com/cli/azure/) |
| **Azure PowerShell** | Azure automation | [learn.microsoft.com/powershell/azure](https://learn.microsoft.com/powershell/azure/) |
| **GitHub CLI (gh)** | GitHub from terminal | [cli.github.com](https://cli.github.com/) |
| **dbt CLI** | Data transformations | [docs.getdbt.com](https://docs.getdbt.com/docs/core/installation-overview) |
| **Databricks CLI** | Databricks automation | [docs.databricks.com/cli](https://docs.databricks.com/dev-tools/cli/) |
| **kubectl** | Kubernetes management | [kubernetes.io/kubectl](https://kubernetes.io/docs/reference/kubectl/) |
| **Helm** | Kubernetes package manager | [helm.sh](https://helm.sh/) |
| **Terraform CLI** | Infrastructure as code | [terraform.io/cli](https://developer.hashicorp.com/terraform/cli) |

---

## API & Testing Tools

| Tool | Focus | Link |
|------|-------|------|
| **Postman** | API development & testing | [postman.com](https://www.postman.com/) |
| **Bruno** | Open-source API client | [usebruno.com](https://www.usebruno.com/) |
| **Playwright** | E2E browser testing | [playwright.dev](https://playwright.dev/) |
| **Selenium** | Browser automation | [selenium.dev](https://www.selenium.dev/) |
| **k6** | Load testing | [k6.io](https://k6.io/) |
| **JMeter** | Performance testing | [jmeter.apache.org](https://jmeter.apache.org/) |

---

## Data Quality & Observability

| Tool | Type | Link |
|------|------|------|
| **Great Expectations** | Data validation framework | [greatexpectations.io](https://greatexpectations.io/) |
| **dbt Tests** | SQL-based testing | [docs.getdbt.com/tests](https://docs.getdbt.com/docs/build/data-tests) |
| **Soda Core** | Quality monitoring (OSS) | [soda.io](https://www.soda.io/) |
| **Pandera** | DataFrame validation | [pandera.readthedocs.io](https://pandera.readthedocs.io/) |
| **Monte Carlo** | Data observability | [montecarlodata.com](https://www.montecarlodata.com/) |
| **Bigeye** | Quality monitoring | [bigeye.com](https://www.bigeye.com/) |
| **Datafold** | Data diff & quality | [datafold.com](https://www.datafold.com/) |
| **ydata-profiling** | Automated profiling | [github.com/ydataai](https://github.com/ydataai/ydata-profiling) |
| **OpenRefine** | Data cleansing | [openrefine.org](https://openrefine.org/) |

---

## Data Catalog & Governance

| Tool | Scope | Link |
|------|-------|------|
| **Microsoft Purview** | Azure/Fabric governance | [docs.microsoft.com/purview](https://docs.microsoft.com/azure/purview/) |
| **Unity Catalog** | Databricks governance | [docs.databricks.com/unity-catalog](https://docs.databricks.com/data-governance/unity-catalog/) |
| **Atlan** | Modern data catalog | [atlan.com](https://atlan.com/) |
| **Alation** | Enterprise data catalog | [alation.com](https://www.alation.com/) |
| **Collibra** | Data intelligence | [collibra.com](https://www.collibra.com/) |
| **OpenMetadata** | Discovery & lineage (OSS) | [open-metadata.org](https://open-metadata.org/) |
| **DataHub** | Metadata platform (OSS) | [datahubproject.io](https://datahubproject.io/) |
| **Apache Atlas** | Metadata governance (OSS) | [atlas.apache.org](https://atlas.apache.org/) |
| **OpenLineage** | Lineage standard | [openlineage.io](https://openlineage.io/) |

---

## Container Tools

| Tool | Focus | Link |
|------|-------|------|
| **Docker Desktop** | Container development | [docker.com/desktop](https://www.docker.com/products/docker-desktop/) |
| **Podman** | Daemonless containers | [podman.io](https://podman.io/) |
| **Rancher Desktop** | Kubernetes on desktop | [rancherdesktop.io](https://rancherdesktop.io/) |
| **Lens** | Kubernetes IDE | [k8slens.dev](https://k8slens.dev/) |

---

## Code Security & Scanning

| Tool | Focus | Link |
|------|-------|------|
| **SonarQube** | Code quality & security | [sonarqube.org](https://www.sonarqube.org/) |
| **SonarCloud** | Cloud code analysis | [sonarcloud.io](https://sonarcloud.io/) |
| **Snyk** | Dependency & container security | [snyk.io](https://snyk.io/) |
| **Trivy** | Container vulnerability scanner | [trivy.dev](https://trivy.dev/) |
| **Checkov** | IaC security scanning | [checkov.io](https://www.checkov.io/) |
| **OWASP ZAP** | Web app security testing | [zaproxy.org](https://www.zaproxy.org/) |
| **GitHub Dependabot** | Dependency updates | [github.com/dependabot](https://github.com/dependabot) |

---

## Diagramming

| Tool | Focus | Link |
|------|-------|------|
| **diagrams.net** | Free diagramming | [diagrams.net](https://www.diagrams.net/) |
| **Visio** | Microsoft diagramming | [microsoft.com/visio](https://www.microsoft.com/microsoft-365/visio/flowchart-software) |
| **Lucidchart** | Collaborative diagrams | [lucidchart.com](https://www.lucidchart.com/) |
| **Miro** | Whiteboarding | [miro.com](https://miro.com/) |
| **PlantUML** | Diagrams as code | [plantuml.com](https://plantuml.com/) |
| **Mermaid** | Markdown diagrams | [mermaid.js.org](https://mermaid.js.org/) |
| **Excalidraw** | Hand-drawn diagrams | [excalidraw.com](https://excalidraw.com/) |

---

## Work Tracking

| Tool | Focus | Link |
|------|-------|------|
| **Azure Boards** | Azure DevOps work tracking | [azure.microsoft.com/devops/boards](https://azure.microsoft.com/services/devops/boards/) |
| **Jira** | Agile project management | [atlassian.com/jira](https://www.atlassian.com/software/jira) |
| **Microsoft Project** | Traditional PM | [microsoft.com/project](https://www.microsoft.com/microsoft-365/project/project-management-software) |
| **Linear** | Modern issue tracking | [linear.app](https://linear.app/) |
| **Notion** | All-in-one workspace | [notion.so](https://www.notion.so/) |

---

## Documentation & Collaboration

| Tool | Focus | Link |
|------|-------|------|
| **Microsoft Teams** | Team collaboration | [microsoft.com/teams](https://www.microsoft.com/microsoft-teams/) |
| **SharePoint** | Document management | [microsoft.com/sharepoint](https://www.microsoft.com/microsoft-365/sharepoint/) |
| **Confluence** | Team documentation | [atlassian.com/confluence](https://www.atlassian.com/software/confluence) |
| **Obsidian** | Personal knowledge base | [obsidian.md](https://obsidian.md/) |

---

## Quick Reference

| Category | Recommended |
|----------|-------------|
| **Editor** | VS Code + GitLens + Bicep |
| **Database** | Azure Data Studio, SSMS |
| **CLI** | Azure CLI, dbt CLI, kubectl |
| **API Testing** | Postman or Bruno |
| **Data Quality** | Great Expectations, dbt Tests, Soda |
| **Catalog** | Purview (Azure) or Unity Catalog (Databricks) |
| **Security** | Snyk, SonarCloud, Trivy |
| **Diagrams** | diagrams.net, Mermaid |

---

> **Note**: For cloud services (ADF, Databricks, Fabric), see [concepts/cloud/](../concepts/cloud/).
