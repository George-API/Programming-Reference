# Terminology Reference

**Related**: [References](resources/References.md) | [Tools](resources/Tools.md)

---

## Cloud & Infrastructure

- **API Gateway**: Centralized entry point for API requests; handles routing, authentication, rate limiting
- **CDN (Content Delivery Network)**: Distributed network for delivering content with low latency
- **CI/CD (Continuous Integration/Deployment)**: Automated build, test, and deployment pipelines
- **Circuit Breaker**: Pattern to prevent cascading failures by stopping requests to failing services
- **Container Orchestration**: Managing containerized applications (Kubernetes, Docker Swarm)
- **DDoS Protection**: Distributed Denial of Service protection at network edge
- **DNS (Domain Name System)**: System translating domain names to IP addresses
- **DR (Disaster Recovery)**: Strategy and processes for recovering IT systems after catastrophic failure
- **Event-Driven Architecture**: Architecture pattern using events for communication between services
- **GitOps**: Operational model using Git as source of truth for infrastructure
- **HA (High Availability)**: System design ensuring continuous operation with minimal downtime
- **Horizontal Scaling**: Adding more instances/nodes to handle increased load (scale out)
- **IaC (Infrastructure as Code)**: Managing infrastructure through code/configuration (Terraform, Bicep)
- **Immutable Infrastructure**: Infrastructure replaced rather than modified; ensures consistency
- **Kubernetes**: Container orchestration platform for automated deployment and scaling
- **Load Balancer**: Distributes traffic across multiple servers
- **Managed Identity**: Cloud-native identity for resources (no secrets required)
- **mTLS (Mutual TLS)**: Mutual authentication using TLS certificates
- **Multi-tenant SaaS**: Software architecture serving multiple customers from single instance
- **NAT (Network Address Translation)**: Translating private IPs to public IPs
- **OIDC (OpenID Connect)**: Authentication protocol built on OAuth 2.0
- **Private Link**: Secure connectivity to cloud services without public internet
- **Rolling Deployment**: Gradual replacement of instances with new versions (zero downtime)
- **RPO (Recovery Point Objective)**: Maximum acceptable data loss measured in time
- **RTO (Recovery Time Objective)**: Maximum acceptable downtime after failure
- **Serverless**: Cloud computing model where provider manages infrastructure
- **Service Mesh**: Infrastructure layer for managing service-to-service communication
- **Vertical Scaling**: Increasing resources (CPU, RAM) on existing instance (scale up)
- **VNet (Virtual Network)**: Isolated network environment in cloud
- **WAF (Web Application Firewall)**: Firewall protecting web applications from attacks
- **Zero Trust**: Security model requiring verification for every access request

---

## Networking & Protocols

- **Bandwidth**: Maximum data transfer capacity of a network connection
- **CORS (Cross-Origin Resource Sharing)**: HTTP mechanism allowing cross-domain requests
- **Firewall**: Network security system controlling incoming/outgoing traffic
- **GraphQL**: Query language for APIs; client specifies exact data needed
- **HTTP/HTTPS**: Hypertext Transfer Protocol; HTTPS adds TLS encryption
- **IP Address**: Unique numerical identifier for devices on a network
- **Latency**: Time delay between request and response
- **Load Balancer**: Distributes traffic across multiple servers for availability
- **OSI Model**: Seven-layer networking model (Physical, Data Link, Network, Transport, Session, Presentation, Application)
- **Proxy**: Intermediary server between client and destination server
- **Rate Limiting**: Controlling number of requests a client can make in a time period
- **REST (Representational State Transfer)**: Architectural style for web APIs using HTTP methods
- **Reverse Proxy**: Server that forwards client requests to backend servers (nginx, HAProxy)
- **Subnet**: Logical subdivision of an IP network
- **TCP (Transmission Control Protocol)**: Reliable, ordered, connection-oriented protocol
- **Throughput**: Actual data transferred per unit of time
- **UDP (User Datagram Protocol)**: Fast, connectionless protocol; no delivery guarantee
- **WebSocket**: Protocol for full-duplex, persistent connections between client and server

---

## Azure & Microsoft Data Platform

- **ADF (Azure Data Factory)**: Cloud-based data integration and orchestration service
- **ADLS Gen2 (Azure Data Lake Storage)**: Scalable data lake storage with hierarchical namespace
- **Delta Lake**: Open-source storage layer with ACID transactions for data lakes
- **Delta Live Tables (DLT)**: Databricks declarative framework for building data pipelines
- **Direct Lake**: Power BI mode querying OneLake Delta tables directly (no import/DirectQuery)
- **Medallion Architecture**: Bronze (raw) → Silver (cleaned) → Gold (curated) data layers
- **OneLake**: Microsoft Fabric unified data lake storage across workspaces
- **Parquet**: Columnar storage format optimized for analytics workloads
- **Photon Engine**: Databricks accelerated SQL engine for faster query performance
- **Real-Time Intelligence**: Fabric workload for streaming data processing
- **Shortcuts**: Fabric references to external data without copying
- **Structured Streaming**: Spark API for stream processing with exactly-once semantics
- **Synapse Analytics**: Analytics service combining data warehousing and big data
- **Unity Catalog**: Databricks unified governance layer for data assets
- **Workspace**: Databricks/Fabric environment for notebooks, jobs, clusters

---

## Data Engineering & Architecture

- **At-Least-Once Delivery**: Message delivered one or more times; requires idempotent consumers
- **Backpressure**: Mechanism to slow producers when consumers can't keep up
- **Batch Processing**: Processing data in scheduled batches (hourly, daily)
- **CDC (Change Data Capture)**: Capturing incremental changes from source systems
- **Data Contract**: Agreement defining schema, semantics, and SLAs for data products
- **Data Drift**: Unexpected changes in data distribution or schema over time
- **Data Fabric**: Architecture pattern providing unified data access across environments
- **Data Lake**: Centralized repository storing raw data in native format
- **Data Lakehouse**: Architecture combining data lake flexibility with warehouse structure
- **Data Lineage**: Tracking data flow from source to consumption
- **Data Mesh**: Decentralized data architecture with domain-oriented ownership
- **Data Product**: Self-contained, reusable data asset with defined contract
- **Data Warehouse**: Centralized repository for structured, queryable data
- **DLQ (Dead Letter Queue)**: Queue for messages that fail processing; enables retry/investigation
- **ELT (Extract, Load, Transform)**: Load raw data first, transform in destination
- **ETL (Extract, Transform, Load)**: Transform data before loading to destination
- **Exactly-Once Delivery**: Message processed exactly once; requires coordination
- **Idempotency**: Operation that produces same result regardless of execution count
- **Incremental Processing**: Processing only new/changed data since last run
- **Lambda Architecture**: Architecture combining batch and streaming processing
- **Metadata**: Data about data (schema, lineage, quality, usage statistics)
- **Orchestration**: Coordinating multiple data pipeline tasks (Airflow, ADF, Prefect)
- **Schema Evolution**: Managing schema changes while maintaining compatibility
- **Schema-on-Read**: Schema applied when reading data (flexible, late binding)
- **Schema-on-Write**: Schema enforced when writing data (strict, early binding)
- **Star Schema**: Data warehouse schema with central fact tables and dimension tables
- **Streaming Processing**: Real-time continuous data processing
- **Watermarking**: Tracking high-water mark for incremental data loads

---

## Data Quality & Operations

- **Data Catalog**: Inventory of data assets with metadata and discovery
- **Data Freshness**: Measure of how current data is (time since last update)
- **Data Observability**: Monitoring data health, quality, and lineage across systems
- **Data Profiling**: Analyzing data to understand structure, content, and quality issues
- **Data Quality**: Measure of data accuracy, completeness, consistency, timeliness
- **Data Reliability Engineering**: Practice of ensuring data systems meet quality and availability targets
- **Data SLO (Service Level Objective)**: Targets for data freshness, completeness, accuracy
- **Data Steward**: Person responsible for data quality and governance within a domain
- **Data Validation**: Checking data against rules, constraints, and expectations
- **Golden Record**: Single authoritative version of an entity across systems
- **MDM (Master Data Management)**: Processes ensuring consistent, accurate master data across systems
- **Quarantine**: Isolating bad/invalid data for review and remediation

---

## Database & Security

- **ACID**: Atomicity, Consistency, Isolation, Durability (transaction properties)
- **CLS (Column-Level Security)**: Restricting access to specific columns
- **Encryption at Rest**: Protecting stored data through encryption
- **Encryption in Transit**: Protecting data during transmission (TLS/SSL)
- **IAM (Identity and Access Management)**: Framework for managing digital identities and access
- **Index**: Database structure improving query performance
- **MFA (Multi-Factor Authentication)**: Authentication requiring multiple verification methods
- **Partitioning**: Dividing large tables into smaller, manageable pieces
- **PII (Personally Identifiable Information)**: Data that can identify individuals
- **Query Optimization**: Improving database query performance through execution plans
- **RBAC (Role-Based Access Control)**: Access permissions based on user roles
- **RLS (Row-Level Security)**: Restricting access to specific rows based on user context
- **Secrets Management**: Secure storage and access control for credentials, keys, tokens
- **SSO (Single Sign-On)**: Authentication allowing access to multiple systems with one login
- **Transaction**: Unit of work that must complete entirely or not at all

---

## Software Architecture & Design

- **API Versioning**: Managing multiple API versions for backward compatibility
- **Bounded Context**: Explicit boundary where domain model applies (DDD concept)
- **Bulkhead Pattern**: Isolating components to prevent cascading failures
- **CQRS (Command Query Responsibility Segregation)**: Separating read and write operations
- **DDD (Domain-Driven Design)**: Software design approach focusing on domain model
- **Design Pattern**: Reusable solution to common software design problems
- **Event Sourcing**: Storing state changes as sequence of events
- **Hexagonal Architecture**: Architecture isolating business logic from infrastructure
- **Microservices**: Architecture pattern with independently deployable services
- **Monolith**: Single deployable unit containing all application functionality
- **Outbox Pattern**: Reliable event publishing using database transactions
- **Retry Pattern**: Automatically retrying failed operations with backoff
- **Saga Pattern**: Managing distributed transactions across services using compensating actions
- **SOLID Principles**: Single responsibility, Open-closed, Liskov substitution, Interface segregation, Dependency inversion

---

## Data Structures & Algorithms

- **Array**: Contiguous memory storing elements of same type; O(1) access by index
- **BFS (Breadth-First Search)**: Graph traversal exploring neighbors before children
- **Big O Notation**: Describes algorithm time/space complexity as input grows
- **Binary Search**: Finding element in sorted array by halving search space; O(log n)
- **Binary Tree**: Tree where each node has at most two children
- **DFS (Depth-First Search)**: Graph traversal exploring as deep as possible before backtracking
- **Dynamic Programming**: Solving problems by breaking into overlapping subproblems; memoization
- **Graph**: Data structure with nodes (vertices) and edges; models relationships
- **Greedy Algorithm**: Making locally optimal choices at each step
- **Hash Table/HashMap**: Key-value store with O(1) average lookup using hash function
- **Heap**: Tree-based structure where parent is greater/less than children; priority queue
- **Linked List**: Linear structure where elements point to next; O(1) insert, O(n) access
- **Memoization**: Caching function results to avoid redundant computation
- **Queue**: FIFO (First In, First Out) data structure
- **Recursion**: Function calling itself to solve smaller subproblems
- **Sliding Window**: Technique for processing subarrays/substrings efficiently
- **Space Complexity**: Memory usage of algorithm as function of input size
- **Stack**: LIFO (Last In, First Out) data structure
- **Time Complexity**: Execution time of algorithm as function of input size
- **Tree**: Hierarchical structure with root node and children; no cycles
- **Trie**: Tree for storing strings; enables prefix search
- **Two Pointers**: Technique using two indices to traverse array efficiently

---

## DevOps & Operations

- **Blue-Green Deployment**: Deployment strategy with two identical environments
- **Canary Deployment**: Gradual rollout to subset of users before full release
- **Configuration Drift**: Unplanned divergence between infrastructure state and definition
- **Feature Flag**: Toggle enabling/disabling features without code deployment
- **MTBF (Mean Time Between Failures)**: Average time between consecutive failures
- **MTTD (Mean Time To Detection)**: Average time to detect a failure or incident
- **MTTF (Mean Time To Failure)**: Average time until first failure
- **MTTR (Mean Time To Recovery)**: Average time to recover from failure
- **Observability**: Ability to understand system state from external outputs (logs, metrics, traces)
- **Platform Engineering**: Practice of building internal developer platforms and tooling
- **Runbook**: Documented procedures for routine operations and incident response
- **Shift Left**: Moving testing, security, and quality earlier in development lifecycle
- **Shift Right**: Testing and monitoring in production environments
- **SRE (Site Reliability Engineering)**: Practice applying software engineering to operations problems
- **Toil**: Repetitive, manual, automatable work that scales with service growth

---

## Security & Compliance

- **CSPM (Cloud Security Posture Management)**: Continuous monitoring of cloud security configuration
- **CWPP (Cloud Workload Protection Platform)**: Security platform protecting cloud workloads
- **DAST (Dynamic Application Security Testing)**: Testing running applications for vulnerabilities
- **Least Privilege**: Granting minimum permissions necessary to perform a function
- **SAST (Static Application Security Testing)**: Analyzing source code for security vulnerabilities
- **SBOM (Software Bill of Materials)**: Inventory of components and dependencies in software
- **SCA (Software Composition Analysis)**: Identifying vulnerabilities in open-source dependencies
- **SIEM (Security Information and Event Management)**: Centralized security event monitoring
- **SOAR (Security Orchestration, Automation and Response)**: Automating security operations
- **Supply Chain Security**: Protecting software supply chain from vulnerabilities and attacks
- **Threat Modeling**: Identifying potential security threats and mitigations

---

## AI, ML & Data Science

- **A/B Testing**: Comparing two variants to determine which performs better
- **Agentic AI**: AI systems that can autonomously plan and execute multi-step tasks
- **AI Governance**: Framework for managing AI systems responsibly and ethically
- **Attention Mechanism**: Neural network component focusing on relevant input parts
- **Bias (ML)**: Systematic error from assumptions; underfitting
- **Classification**: Predicting discrete categories (spam/not spam)
- **CNN (Convolutional Neural Network)**: Neural network for image/spatial data processing
- **Confusion Matrix**: Table showing true/false positives and negatives
- **Cross-Validation**: Evaluating model on different data subsets to assess generalization
- **Deep Learning**: Neural networks with multiple layers for complex pattern recognition
- **Embedding**: Vector representation of data (text, images) for ML models
- **Epoch**: One complete pass through entire training dataset
- **Feature Engineering**: Creating/selecting input variables to improve model performance
- **Feature Store**: Centralized repository for ML features; enables reuse and consistency
- **Gradient Descent**: Optimization algorithm minimizing loss by following gradient
- **Grounding**: Connecting AI responses to factual, verifiable information
- **Hallucination**: AI generating plausible but incorrect or fabricated information
- **Hyperparameter Tuning**: Optimizing model configuration (learning rate, layers, etc.)
- **Inference**: Using trained model to make predictions on new data
- **LLM (Large Language Model)**: AI model trained on large text datasets for natural language tasks
- **Loss Function**: Measures difference between predicted and actual values
- **MLOps**: Practices for deploying and maintaining ML models in production
- **Model Drift**: Degradation of model performance over time as data changes
- **Model Fine-tuning**: Adapting pre-trained models for specific tasks or domains
- **Model Registry**: Versioned repository for ML models with metadata
- **Neural Network**: Computing system inspired by biological neurons; learns patterns
- **Overfitting**: Model learns training data too well; poor generalization
- **Precision**: True positives / (True positives + False positives)
- **Prompt Engineering**: Crafting effective prompts to get desired outputs from AI models
- **RAG (Retrieval Augmented Generation)**: AI pattern combining retrieval with generation for accurate responses
- **Recall**: True positives / (True positives + False negatives)
- **Regression**: Predicting continuous values (price, temperature)
- **Supervised Learning**: Training with labeled data (input-output pairs)
- **Token**: Basic unit of text processed by language models
- **Training**: Process of teaching model using data to adjust weights
- **Transformer**: Neural network architecture using self-attention; basis for GPT, BERT
- **Underfitting**: Model too simple to capture underlying patterns
- **Unsupervised Learning**: Training with unlabeled data; finds patterns/clusters
- **Variance (ML)**: Sensitivity to training data fluctuations; overfitting

---

## Product Management

- **A/B Testing**: Comparing product variants to measure impact on metrics
- **CAC (Customer Acquisition Cost)**: Total cost to acquire a new customer
- **Churn Rate**: Percentage of customers who stop using product over time period
- **Cohort Analysis**: Analyzing behavior of user groups over time
- **Customer Journey**: End-to-end experience of customer with product/company
- **Discovery**: Research phase to understand user problems and validate solutions
- **Feature Prioritization**: Deciding which features to build based on value/effort
- **Go-to-Market (GTM)**: Strategy for launching product to target customers
- **Jobs to Be Done (JTBD)**: Framework focusing on outcomes customers want to achieve
- **LTV (Lifetime Value)**: Total revenue expected from a customer over relationship
- **North Star Metric**: Single metric that best captures core value delivered to customers
- **NPS (Net Promoter Score)**: Customer loyalty metric based on likelihood to recommend
- **PLG (Product-Led Growth)**: Growth strategy where product drives acquisition/retention
- **Product Discovery**: Research to understand problems before building solutions
- **Product-Market Fit**: When product satisfies strong market demand
- **Product Vision**: Aspirational description of what product aims to achieve long-term
- **Time to Value (TTV)**: Time for customer to realize value from product
- **User Research**: Methods to understand user needs, behaviors, and motivations
- **Value Proposition**: Statement of benefits product provides to customers

---

## Project Management

- **ADKAR Model**: Change management model (Awareness, Desire, Knowledge, Ability, Reinforcement)
- **Backlog Refinement**: Process of reviewing and updating product backlog
- **Burndown Chart**: Visual representation of work remaining over time
- **Change Control Board (CCB)**: Group responsible for approving project changes
- **Critical Path**: Sequence of dependent tasks determining minimum project duration
- **Definition of Done (DoD)**: Shared understanding of when work is complete
- **Definition of Ready (DoR)**: Criteria a story must meet before sprint planning
- **Dependency**: Relationship where one task must complete before another starts
- **EVM (Earned Value Management)**: Project performance measurement technique
- **Executive Sponsor**: Senior executive who champions and supports the project
- **Gantt Chart**: Visual timeline showing project tasks, durations, and dependencies
- **Kotter's 8-Step Process**: Change management framework for organizational transformation
- **Milestone**: Significant event or achievement in project timeline
- **MoSCoW**: Prioritization method (Must have, Should have, Could have, Won't have)
- **MVP (Minimum Viable Product)**: Simplest product version that delivers core value
- **OKRs (Objectives and Key Results)**: Goal-setting framework linking objectives to measurable results
- **PMO (Project Management Office)**: Organizational unit providing PM standards and support
- **Portfolio Management**: Managing collection of projects to achieve strategic objectives
- **Power/Interest Grid**: Matrix categorizing stakeholders by power and interest levels
- **Program Management**: Coordinated management of related projects to deliver benefits
- **Project Charter**: Document authorizing project and defining objectives
- **RACI Matrix**: Responsibility assignment (Responsible, Accountable, Consulted, Informed)
- **Risk Register**: Document tracking identified risks and responses
- **Roadmap**: High-level strategic view of project goals, milestones, timeline
- **Scope Creep**: Uncontrolled expansion of project scope
- **SMART Objectives**: Specific, Measurable, Achievable, Relevant, Time-bound goals
- **Stage Gate**: Go/no-go decision point at key project milestones
- **Stakeholder**: Individual or group with interest in project outcome
- **Stakeholder Register**: Document identifying stakeholders, interests, and engagement needs
- **Steering Committee**: Executive oversight body making strategic project decisions
- **Technical Debt**: Shortcuts in development that create future maintenance burden
- **Triple Constraint**: Scope, Time, Cost (fundamental project constraints)
- **Vendor Management**: Managing relationships with third-party vendors
- **WBS (Work Breakdown Structure)**: Hierarchical decomposition of project work

---

## Agile & SDLC

- **Acceptance Criteria**: Conditions that must be met for story to be considered complete
- **Agile**: Iterative, incremental development methodology
- **Daily Standup**: Brief daily team synchronization meeting
- **Epic**: Large body of work that can be broken down into smaller stories
- **Increment**: Potentially releasable product at the end of each sprint
- **Kanban**: Visual workflow management method using boards and WIP limits
- **PRINCE2**: Process-based project management methodology
- **Product Manager**: Role responsible for product strategy, roadmap, and feature prioritization
- **Product Owner**: Scrum role owning product backlog, prioritizing work, defining acceptance
- **SAFe (Scaled Agile Framework)**: Framework for scaling Agile to enterprise
- **Scrum**: Agile framework with sprints, roles (PO, SM, Dev), and ceremonies
- **Scrum Master**: Facilitator ensuring Scrum practices are followed; removes impediments
- **SDLC (Software Development Life Cycle)**: Phases of software development from planning to maintenance
- **Spike**: Time-boxed research task to reduce uncertainty or explore solutions
- **Sprint**: Time-boxed iteration in Agile development (typically 2 weeks)
- **Sprint Planning**: Meeting to plan work for upcoming sprint
- **Sprint Retrospective**: Meeting to reflect on team process and improve
- **Sprint Review**: Meeting to demonstrate completed work to stakeholders
- **Story Points**: Relative estimation unit for story complexity
- **User Story**: Short description of feature from user perspective
- **Velocity**: Measure of work completed per sprint
- **Waterfall**: Sequential project management methodology
- **WIP (Work in Progress)**: Number of items currently being worked on; limiting WIP improves flow

---

## Business & Finance

- **Allocation**: Distribution of costs to departments, projects, or cost centers
- **Baseline**: Approved plan used as reference for performance measurement
- **Budget**: Financial plan allocating resources for specific period or project
- **Business Case**: Justification for a project including costs, benefits, and risks
- **CapEx (Capital Expenditure)**: One-time investment in assets (hardware, infrastructure)
- **Chargeback**: Allocating IT costs to business units based on actual usage
- **Cost Center**: Organizational unit that incurs costs but doesn't generate revenue
- **Cost Variance (CV)**: Difference between earned value and actual cost (CV = EV - AC)
- **Depreciation**: Accounting method allocating asset cost over useful life
- **EAC (Estimate At Completion)**: Projected total cost at project completion
- **ETC (Estimate To Complete)**: Projected cost to complete remaining work
- **FinOps**: Financial operations practice optimizing cloud spending
- **KPI (Key Performance Indicator)**: Measurable value demonstrating effectiveness
- **MSA (Master Service Agreement)**: Framework agreement governing multiple projects
- **NPV (Net Present Value)**: Present value of future cash flows minus initial investment
- **OpEx (Operational Expenditure)**: Ongoing operational costs (cloud services, licenses)
- **Profit Center**: Organizational unit that generates revenue and profit
- **RFP (Request for Proposal)**: Document soliciting vendor proposals
- **RFQ (Request for Quote)**: Document requesting pricing information
- **ROI (Return on Investment)**: Measure of profitability: (gain - cost) / cost
- **Showback**: Reporting IT costs to business units without actual charge
- **SLA (Service Level Agreement)**: Contractual commitment to service level
- **SLI (Service Level Indicator)**: Measurable aspect of service performance
- **SLO (Service Level Objective)**: Internal target for service reliability/performance
- **SOW (Statement of Work)**: Detailed description of work to be performed
- **TCO (Total Cost of Ownership)**: Total cost including acquisition, operation, maintenance

---

> **Note**: For implementation patterns, see [concepts/](../concepts/).
