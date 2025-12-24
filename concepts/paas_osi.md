PaaS Best Practices & Common Concepts — OSI Model Lens (Cloud-Focused)
Goal: map what you do in PaaS designs to OSI layers for faster reasoning
about security, connectivity, performance, and troubleshooting.

Legend (common Azure examples):
- Edge/WAF: Azure Front Door, Application Gateway (WAF)
- API: API Management (APIM)
- App: App Service, Functions, Container Apps
- Network: VNet, NSG, Private Link, Azure Firewall, NAT Gateway
- Data: Azure SQL, Cosmos DB, Storage/ADLS, Service Bus, Event Hubs
- Observability: Azure Monitor, Log Analytics, App Insights

------------------------------------------------------------
## Layer 1 — Physical (Bits)
------------------------------------------------------------
Cloud meaning:
- You don’t manage hardware, cables, RF, or physical switching.
Best-practice mindset:
- Treat as “abstracted” but plan for region/zone physical separation.
PaaS implications:
- Choose zone-redundant services where supported.
- Use multi-region DR where business requires.
Troubleshooting hint:
- Most “physical” issues show up as regional incidents/availability events.

------------------------------------------------------------
Layer 2 — Data Link (Frames)
------------------------------------------------------------
Cloud meaning:
- Underlying NIC/VLAN/L2 switching abstracted; you mostly see it via VNet constructs.
PaaS best practices:
- Prefer managed networking; avoid assumptions about L2 adjacency.
- Use proper subnet segmentation in VNets (even for PaaS private endpoints).
Common Azure controls:
- NSGs (stateful L4 filtering at subnet/NIC), VNet peering.
Troubleshooting hint:
- If private endpoint traffic fails, check subnet/NSG + private DNS linkage first.

------------------------------------------------------------
Layer 3 — Network (IP, Routing)
------------------------------------------------------------
What you control in PaaS designs:
- IP addressability, routing boundaries, private/public access posture.
Best practices:
- Default to private access for PaaS data services (Private Link) where possible.
- Centralize routing in hub-spoke or Virtual WAN for enterprise estates.
- Control egress (NAT Gateway / Azure Firewall) when partner allowlists exist.
Common failure modes:
- DNS resolves to public endpoint when you expected private.
- Route tables / peering / firewall blocks expected flows.
Key checks:
- Effective routes, private DNS zones, “public network access” toggles.

------------------------------------------------------------
Layer 4 — Transport (TCP/UDP, Ports, Reliability)
------------------------------------------------------------
PaaS reality:
- Most integration is TCP (HTTPS/AMQP); UDP less common (some DNS scenarios).
Best practices:
- Timeouts everywhere (HTTP client, DB drivers, messaging clients).
- Retries with backoff + jitter (avoid retry storms).
- Circuit breakers + retry budgets (don’t melt dependencies).
- Keep-alives/connection pooling (DB + HTTP).
Azure examples:
- Service Bus (AMQP over TCP), SQL (TDS over TCP), HTTPS everywhere.
Common failure modes:
- Hanging calls (missing timeouts), connection exhaustion (no pooling), port blocks.
Key checks:
- Client timeouts, connection pool sizes, SNAT/egress capacity (NAT).

------------------------------------------------------------
Layer 5 — Session (Connection Semantics)
------------------------------------------------------------
Modern mapping:
- TLS session + authenticated sessions + long-lived connections (AMQP/WebSockets).
Best practices:
- Use TLS everywhere; validate certificates; rotate certs.
- Prefer token-based auth (Entra ID) vs long-lived secrets.
- Avoid “sticky session” assumptions; keep services stateless.
Azure examples:
- Managed Identity token acquisition; Service Bus sessions (ordering semantics).
Common failure modes:
- Token expiry mishandled, re-auth loops, broken session affinity.
Key checks:
- Token refresh, clock skew, TLS handshake errors, session ordering keys.

------------------------------------------------------------
Layer 6 — Presentation (Serialization, Encoding)
------------------------------------------------------------
Modern mapping:
- JSON/Avro/Protobuf/XML, compression, character encodings, schema compatibility.
Best practices:
- Standardize encodings: UTF-8, ISO timestamps, explicit decimals handling.
- Validate schemas at boundaries; reject/route bad payloads to quarantine.
- Version payloads (backward compatible by default).
Azure examples:
- JSON APIs via APIM; XML/SOAP legacy; event payload contracts for Event Grid/Service Bus.
Common failure modes:
- Breaking schema changes, timezone/format drift, encoding issues, float precision loss.
Key checks:
- Schema version, contract tests, serialization settings, date parsing rules.

------------------------------------------------------------
Layer 7 — Application (Business Protocols + PaaS Services)
------------------------------------------------------------
Where most PaaS architecture lives:
- API design, integration patterns, messaging, data access, observability, authZ.
Best practices (core):
- API gateway: auth, throttling, versioning, transformation (light), routing.
- Messaging: at-least-once + idempotency, DLQs, replay tooling, outbox/inbox patterns.
- Data: separate OLTP vs OLAP, medallion layering for analytics, data contracts.
- Observability: correlation IDs, distributed tracing, SLO-based alerting.
Azure examples:
- APIM policies, Functions handlers, Service Bus topics, Event Grid fan-out.
Common failure modes:
- Duplicate processing, poison messages, unbounded fan-out, “chatty” APIs, N+1 DB calls.
Key checks:
- Idempotency keys, DLQ counts, queue depth, error budget burn, dependency latency.

------------------------------------------------------------
Cross-Layer “PaaS Defaults” Checklist (Practical)
------------------------------------------------------------
Security posture
- L3: Prefer private endpoints for data plane + private DNS
- L4: TLS everywhere; restrict ports; timeouts + retries
- L7: Central auth at gateway + app-level authorization checks

Reliability
- L1/L3: zone redundancy + DR tier defined
- L4: bounded concurrency, connection pooling, backpressure
- L7: idempotent consumers + DLQ + replay, circuit breakers

Troubleshooting workflow (fast)
1) L7: Is the app returning errors? check logs/traces (App Insights)
2) L6: Payload/schema/encoding issues? validate contract + sample message
3) L5/L4: Auth/session/timeouts/retries? inspect tokens + client settings
4) L3: DNS/private endpoint/routing? verify name resolution + effective routes
5) L2: NSG/peering? verify allowed flows
6) L1: region incident? check platform health/status


Common “Where does this live in OSI?” Mapping (mental shortcuts)
- WAF / API Gateway policies -> Layer 7
- JWT / OAuth tokens -> Layer 5–7 (session/auth semantics)
- TLS / certificates -> Layer 5/6 (secure session + presentation)
- DNS + private DNS zones -> Layer 3 (name->IP reachability)
- Private Link / routing / firewalls -> Layer 3/4
- Ports / TCP keepalive / pooling -> Layer 4
- JSON/XML/Avro schema evolution -> Layer 6
- Messaging semantics (idempotency, DLQ) -> Layer 7 (application protocol behavior)
