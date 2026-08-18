Modern enterprise architecture is made up of five distinct traffic flows, each serving a different purpose. 

Summary of the 5 Traffic Flows

1. North-South Traffic (External APIs)
    * Handles communication between external users/partners and internal systems.
    * Uses components such as Load Balancers, Reverse Proxies, Web Application Firewalls (WAFs), and API Gateways.
    * Focuses on:
        * Authentication & Authorization
        * Security
        * Rate limiting
        * Routing
        * API policies
        * Monitoring
2. East-West Traffic (Internal Services)
    * Manages communication between microservices inside the organization.
    * Typically uses a Service Mesh.
    * Provides:
        * Secure service-to-service communication (mTLS)
        * Service discovery
        * Traffic management
        * Circuit breaking
        * Observability
3. Event-Driven Traffic
    * Enables asynchronous communication using event streaming platforms like Kafka.
    * Systems publish and consume events independently.
    * Common use cases:
        * Real-time analytics
        * Fraud detection
        * Notifications
        * CRM updates
    * Benefits:
        * Loose coupling
        * High scalability
        * High throughput
4. Legacy Integration Traffic
    * Connects modern applications with legacy enterprise systems such as:
        * Core Banking
        * ERP
        * CRM
        * Mainframes
    * Uses ESBs (Enterprise Service Buses) or integration platforms for:
        * Data transformation
        * Protocol conversion
        * Orchestration
        * Routing
        * Reliability and monitoring
5. AI Traffic
    * A new architecture layer dedicated to AI/LLM applications.
    * Uses an AI Gateway to connect applications with foundation models and vector databases.
    * Responsibilities include:
        * Model routing
        * Prompt/response management
        * Guardrails
        * Token and cost management
        * RAG (Retrieval-Augmented Generation) using vector databases
        * Observability

Key Takeaway

Modern enterprise systems are not built around a single integration approach. Instead, they combine five specialized communication patterns:

Flow	Purpose
North-South	Secure external API access
East-West	Internal microservice communication
Event-Driven	Asynchronous event processing
Legacy Integration	Connect modern apps with legacy systems
AI Traffic	Manage AI/LLM interactions and governance

Overall message: A scalable, resilient enterprise architecture separates these traffic types, allowing each to use the technologies and governance best suited to its purpose—from edge APIs and microservices to event streaming, legacy integration, and AI-powered applications.

![Architecture Diagram](images/EA.png)
