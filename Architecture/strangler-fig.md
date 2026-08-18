The Strangler Fig Pattern is a software modernization strategy used to gradually replace a legacy application with a new one, rather than rewriting the entire system in one go.

The name comes from the strangler fig tree, which grows around a host tree. Over time, the fig becomes stronger while the original tree gradually dies away. Similarly, new software “grows” around the legacy application until the old system can be retired.

⸻

Strangler Fig Pattern

Problem

Large legacy systems (monoliths) that:

* Are difficult to change
* Have high maintenance costs
* Use outdated technologies
* Cannot scale effectively
* Are business-critical, making replacement risky

A complete rewrite (“Big Bang” migration) often:

* Takes years
* Is expensive
* Carries significant risk
* May never be completed

⸻

Solution

Instead of replacing everything at once:

1. Leave the legacy system running.
2. Build new functionality as independent services.
3. Route requests to either the legacy or new service.
4. Gradually migrate features.
5. Remove legacy components once they are no longer used.

Eventually, nothing remains of the old application.

Example Step-by-Step Migration

Step 1 – Identify a Feature/business services

Suppose your monolith contains:

* Customer Management
* Orders
* Payments
* Inventory
* Shipping

⸻

Step 2 – Extract One Service

Perhaps Orders changes frequently.

Create:

Order Service

Now:
```text
                 API Gateway
                     |
        +------------+-------------+
        |                          |
        v                          v
Legacy Monolith           Order Service
```
Requests for Orders go to the new service.

Everything else still uses the monolith.

⸻

Step 3 – Continue Extracting

Next:

* Payments
* Inventory
* Shipping

```text
                 API Gateway
                     |
     +---------------+----------------+
     |               |                |
     v               v                v
 Legacy         Order Service    Payment Service
 Monolith
```

 Monolith

Eventually:

* Customer Service
* Order Service
* Payment Service
* Inventory Service
* Shipping Service

The monolith becomes empty.

⸻

Typical Architecture

```text
                 Internet
                     |
               Load Balancer
                     |
               API Gateway
                     |
        +------------+------------+
        |                         |
        v                         v
 Legacy Application        Microservices
        |                         |
        +------------+------------+
                     |
                 Shared Database
```
Later, the shared database is often split into service-specific databases.

⸻

Routing

The API Gateway decides where requests go.

Example:
```text
/customers/*  -> Legacy
/orders/*     -> Order Service
/payments/*   -> Payment Service
```

Clients do not need to know where the functionality lives.

⸻

Database Migration

This is usually the hardest part.

Data ownership moves one domain at a time.

⸻

Benefits

Lower Risk

Only small pieces change.

⸻

Continuous Delivery

New features can be released without waiting for a complete rewrite.

⸻

Easy Rollback

If a new service fails, requests can often be redirected back to the legacy system.

⸻

Faster Innovation

Teams can use modern technologies for new services while the legacy system continues to run.

⸻

Incremental Modernization

Business continues operating throughout the migration.

⸻

Challenges

Shared Database

Microservices should ideally own their own data. Sharing a database for too long creates tight coupling.

⸻

Distributed Transactions

Operations spanning multiple services become more complex and may require patterns like Saga.

⸻

Routing Complexity

API Gateways and proxies become responsible for directing traffic correctly.

⸻

Monitoring

Requests may flow through several services, making distributed tracing and centralized logging essential.

⸻

Common Technologies

Purpose	Technologies
API Gateway	Kong, NGINX, Traefik, Azure API Management
Reverse Proxy	NGINX, HAProxy, Envoy
Service Mesh	Istio, Linkerd
Messaging	Kafka, RabbitMQ, Azure Service Bus
Monitoring	Prometheus, Grafana
Tracing	Jaeger, Zipkin, OpenTelemetry

⸻

Real-World Example

Imagine an online retailer with a 15-year-old monolithic application.

Current state:

* All functionality is deployed as one application.
* Any code change requires redeploying the entire system.

Migration plan:

1. Extract Product Catalog into a microservice.
2. Extract Search into its own service.
3. Extract Checkout.
4. Extract Payments.
5. Extract Shipping.
6. Decommission the monolith once all functionality has been migrated.

Customers experience little to no disruption because requests are routed seamlessly between the legacy application and the new services.

⸻

When to Use the Strangler Fig Pattern

This pattern is a good fit when:

* The application is large and business-critical.
* Downtime must be minimized.
* A complete rewrite is too risky or expensive.
* Teams want to modernize incrementally.
* You’re moving from a monolith to microservices or cloud-native architecture.

It may not be the best choice for small applications that can be rewritten quickly, or systems that are already modular and easy to replace.

Key Takeaway

The Strangler Fig Pattern allows you to modernize a legacy system safely by replacing it piece by piece. By introducing an API gateway or routing layer, you can direct traffic to newly built services while the remaining functionality continues to run in the legacy application. Over time, the legacy system shrinks until it can be retired, reducing migration risk while enabling continuous delivery and business continuity.

## Atternative Patterns 
Alternative migration and modernization strategies, each with different trade-offs.

| Pattern | Best For | Risk | Downtime | Complexity |
|---------|----------|------|----------|------------|
| **Strangler Fig** | Incremental modernization | Low | None | Medium |
| **Big Bang Rewrite** | Small or simple applications | Very High | High | Low |
| **Branch by Abstraction** | Refactoring code internally | Low | None | Medium |
| **Anti-Corruption Layer (ACL)** | Integrating legacy with modern systems | Low | None | Medium |
| **Parallel Run** | High-risk systems requiring validation | Low | None | High |
| **Blue-Green Deployment** | Deploying new application versions | Low | Minimal | Medium |
| **Canary Release** | Gradual production rollout | Low | None | Medium |
| **Feature Toggles** | Controlled feature rollout | Low | None | Medium |
| **Facade Pattern** | Hiding legacy complexity | Low | None | Low |
| **Lift and Shift** | Moving to the cloud without redesign | Medium | Low | Low |
| **Replatforming** | Cloud migration with minor improvements | Medium | Low | Medium |
| **Re-architecting** | Cloud-native transformation | Medium | Low | High |



⸻

1. Big Bang Rewrite

Description

Replace the entire application in a single deployment.

Old System
      │
      ▼
Complete Rewrite
      │
      ▼
New System

Advantages

* Clean design from scratch
* No coexistence with legacy code

Disadvantages

* Very high risk
* Long development cycle
* Difficult rollback
* Significant testing effort

Suitable for: Small applications or when the legacy system is no longer viable.

⸻


4. Parallel Run

Run old and new systems simultaneously.

Users
   │
   ▼
Old System
      │
      ├── Compare Results
      │
New System

Advantages

* Easy validation
* Reduced business risk

Disadvantages

* Double infrastructure costs
* Data synchronization challenges

Common in banking, insurance, and healthcare.

⸻

5. Blue-Green Deployment

Maintain two production environments.

Users
Blue (Current)
Green (New)
Switch Traffic

Advantages

* Instant rollback
* Near-zero downtime

Not intended for modernization

It’s primarily a deployment strategy rather than a migration strategy.

⸻

6. Canary Release

Release to a small percentage of users first.

Users
5% → New Version
95% → Old Version

Gradually increase traffic if successful.

Ideal for:

* Kubernetes
* Azure App Service
* AWS ECS
* Service Meshes (Istio, Linkerd)

⸻



9. Lift and Shift

Move the application to cloud infrastructure without redesign.

On-Prem VM
↓
Azure VM

Advantages:

* Fast migration
* Low code changes

Disadvantages:

* Doesn’t solve architectural issues

⸻

10. Replatforming

Move to modern infrastructure with minimal application changes.

Examples:

* SQL Server → Azure SQL Managed Instance
* IIS → Azure App Service
* Docker on VMs → AKS

⸻

11. Re-architecting

Redesign the application for cloud-native technologies.

Monolith
↓
Microservices
↓
Containers
↓
Kubernetes

Often includes:

* Event-driven architecture
* CQRS
* API Gateway
* Service Mesh

⸻

Choosing the Right Pattern

Scenario	Recommended Pattern
Replace a large monolith gradually	Strangler Fig
Hide legacy complexity	Facade
Protect new services from legacy models	Anti-Corruption Layer
Refactor code internally	Branch by Abstraction
Validate a new system against the old	Parallel Run
Deploy with minimal downtime	Blue-Green
Test new releases safely	Canary Release
Roll out features incrementally	Feature Toggles
Move to the cloud quickly	Lift and Shift
Modernize infrastructure	Replatforming
Build a cloud-native solution	Re-architecting
Replace a small application entirely	Big Bang Rewrite

Patterns Often Used Together

These patterns are frequently combined rather than used in isolation. For example:

* Strangler Fig + API Gateway to route requests between legacy and new services.
* Strangler Fig + Anti-Corruption Layer to translate between legacy and modern domain models.
* Strangler Fig + Feature Toggles to control when new functionality is exposed.
* Strangler Fig + Canary Releases to gradually shift traffic to newly extracted services.
* Re-architecting + Event-Driven Architecture using messaging platforms such as Kafka to decouple services.

For your GitHub architecture repository, it would make sense to create an architecture/ folder with one Markdown page per pattern (e.g., strangler-fig.md, anti-corruption-layer.md, branch-by-abstraction.md, canary-release.md, etc.), and cross-link them. This gives you a navigable reference library where readers can easily compare patterns and understand when to use each one.
