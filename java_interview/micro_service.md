### 1. API Gateway
This pattern creates a single, central entry point for all client requests, routing them to the appropriate microservice. 

- Benefits: Reduces client complexity by consolidating multiple API calls, offloads cross-cutting concerns like authentication and rate-limiting, and enables microservice refactoring without impacting clients.

- Considerations: Can become a single point of failure if not properly managed. It is best to have multiple, fine-grained gateways for different client types or business domains. 

### 2. Service Discovery
In a dynamic microservice environment, service instances are frequently scaled up or down, making their network locations unreliable. 
- Benefits: Enables services to find and communicate with each other dynamically without hardcoding addresses.
How it works: A service registry (e.g., Consul, Eureka) maintains a list of available service instances and their locations. Services either register themselves (client-side discovery) or are registered by a load balancer (server-side discovery). 

### 3. Database per Service
This pattern ensures that each microservice has its own private database, preventing schema changes in one service from affecting others. 

- Benefits: Promotes loose coupling, allows for independent scaling, and enables each service to choose the database technology best suited for its needs (polyglot persistence).

- Challenges: Can complicate business transactions and queries that span multiple services. 

### 4. Saga
A saga is a sequence of local transactions that coordinates business processes spanning multiple microservices to maintain data consistency. 

- How it works: If a step in the sequence fails, the saga executes compensating transactions to undo the changes made by previous steps.

- Implementations: Can be coordinated via choreography, where services communicate by exchanging events, or orchestration, where a central orchestrator tells each service what to do. 

### 5. Circuit Breaker
This fault-tolerance pattern prevents cascading failures by halting requests to a failing service. 

- How it works: It has three states:
  1. Closed: Normal operations.
  2. Open: After a failure threshold is exceeded, the circuit trips, and requests fail immediately.
  3. Half-Open: After a timeout, it allows a limited number of test requests to check if the service has recovered. 

### 6. Event-Driven Architecture (EDA)
In this pattern, microservices communicate asynchronously by publishing and consuming events, promoting loose coupling and scalability. 
- Benefits: A failure in one service does not block others, improving resilience. It also supports parallel processing and real-time responsiveness.
- How it works: An event producer publishes a state-change event to a message broker (e.g., Kafka, RabbitMQ). The event is then routed to any interested consumers. 

### 7. Command Query Responsibility Segregation (CQRS)
CQRS separates the data modification commands (writes) from the data retrieval queries (reads), allowing each to be optimized independently. 
- Benefits: Improves performance and scalability, especially in systems with a high volume of reads. The read and write models can use different database technologies.
- Considerations: Increases system complexity and often pairs with the Event Sourcing pattern. 

### 8. Strangler Fig
This pattern facilitates the incremental migration from a monolithic application to a microservice architecture. 
- How it works: A proxy or API gateway is placed in front of the monolith. As new microservices are developed, the proxy redirects requests from the old system to the new services. Over time, the new services "strangle" the monolith until it can be retired. 

### 9. Backends for Frontends (BFF)
A variant of the API Gateway pattern, BFF creates a dedicated backend service for each client application (e.g., web, mobile). 
- Benefits: Optimizes the API and data format specifically for each frontend, simplifying the client-side code and reducing network latency.
- Considerations: Can lead to code duplication across different BFFs. 

### 10. Bulkhead
Inspired by the compartments in a ship's hull, this pattern isolates services and resources to prevent one component's failure from affecting others. 
- How it works: By dedicating separate thread pools, queues, or database connections to different services, a failure or slowdown in one service exhausts only its own resources, leaving other parts of the system unaffected. 

### 11. Sidecar
This pattern deploys a companion container or service alongside a main microservice. 
- Benefits: Handles cross-cutting concerns like logging, monitoring, and security independently, allowing the main service to focus on its core business logic. It also promotes code reuse and simplifies development.
- Example: An Envoy proxy running as a sidecar in a Kubernetes pod to manage network traffic for its main application. 

### 12. Health Check
This pattern adds an endpoint (e.g., /health) to each microservice that reports its status. 
- How it works: Load balancers, service registries, and container orchestrators regularly query this endpoint. If a service instance is unhealthy, traffic can be diverted to a healthy instance, improving overall system reliability and availability. 
AI responses may include mistakes. Learn more




### What other ways can microservices communicate besides REST APIs and message brokers?

### Elaborate on the challenges of distributed transactions and how the Saga pattern addresses them

### Give examples of when to use choreography vs orchestration for sagas

#### Choreography (Event-driven, Decentralized)

When to Use:

Simple, linear workflows
High autonomy between services
Event-driven architecture already in place
Teams prefer loose coupling

Examples:
1. E-commerce Order Processing
   1. Order Created 
   2. Payment Service processes 
   3. Inventory Service reserves 
   4. Shipping Service creates label
Each service listens to events and knows what to do next.
2. User Registration Flow
   1. User Registered 
   2. Email Service sends welcome 
   3. Profile Service creates profile 
   4. Analytics Service tracks signup
3. Content Publishing Pipeline
    1. Article Submitted 
    2. Moderation Service reviews 
    3. Search Service indexes 
    4. Notification Service alerts subscribers

#### Orchestration (Centralized Coordinator)
When to Use:

Complex business workflows
Need centralized monitoring/tracking
Conditional branching logic
Tight SLAs and error handling requirements

Examples:
#### 1. Insurance Claim Processing
java// Complex workflow with multiple decision points
ClaimOrchestrator:
   1. Validate claim
   2. If amount > $10K → require manager approval
   3. Check fraud indicators
   4. If suspicious → trigger investigation
   5. Calculate payout
6. Process payment
7. Update customer records
8. Generate reports
#### 2. Loan Application Process LoanOrchestrator:
1. Credit check
2. Employment verification  
3. If credit < 650 → require co-signer
4. Asset verification
5. Risk assessment
6. If approved → generate documents
7. Schedule closing
#### 3. Travel Booking Saga
javaTravelOrchestrator:
1. Reserve flight
2. If flight fails → return error
3. Reserve hotel
4. If hotel fails → cancel flight
5. Book car rental
6. If car fails → cancel (hotel + flight)
7. Process payment
8. Send confirmation
Decision Matrix

Factor | Choreography| Orchestration
|------|-------------|-------------|
Complexity|Simple linear flows |Complex, branching workflows|
Monitoring| Distributed, harder to track|Centralized, easier to monitor|
Error Handling|Each service handles its own|Centralized error handling|
Performance|Lower latency (no coordinator)|Higher latency (coordinator overhead)|
Scalability|Better (no single point)|Coordinator can be bottleneck|
Testing|Harder (end-to-end)|Easier (test coordinator logic)|

#### Real-World Hybrid Example
#### Netflix Video Processing Pipeline
```
java// Choreography for main flow
Video Upload → Transcoding Service → Quality Check → CDN Distribution

// Orchestration for complex scenarios
TranscodingOrchestrator:
1. Detect video format
2. If 4K → use high-performance nodes
3. If live stream → priority queue
4. If error rate > 5% → retry with different codec
5. Generate multiple resolutions
6. Update metadata service
```

#### Key Takeaways
#### Choose Choreography when:

- Simple, predictable workflows
- High service autonomy desired
- Event-driven architecture exists
- Performance is critical

#### Choose Orchestration when:

- Complex business logic
- Need visibility and control
- Multiple conditional paths
- Strict compliance requirements
- Compensation logic is complex

Hybrid Approach:
Many organizations use both - choreography for simple flows within bounded contexts, orchestration for complex cross-domain workflows.