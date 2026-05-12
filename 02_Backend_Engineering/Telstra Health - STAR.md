---
created: 2026-05-13 07:41
updated: 2026-05-13 07:41
tags:
  - backend
status: active
language:
source:
related:
url:
---


> [!info]
> Backend engineering concept or implementation pattern.


# Story Title

## Designing a Reliable Event-Driven Participant Orchestration Platform for Australia’s Lung Cancer Screening Program

---

# Why This Story Is Strong

This story demonstrates:

- Distributed systems engineering
    
- Event-driven architecture
    
- Kafka-based async processing
    
- Spring Boot microservices
    
- Kubernetes / AWS EKS
    
- Idempotency and reliability engineering
    
- Legacy modernization
    
- Workflow orchestration with Camunda
    
- Cross-system consistency
    
- Healthcare domain complexity
    
- Staff-level architectural tradeoff thinking
    
- Production-grade resiliency patterns
    

---

# STAR FORMAT

# S — Situation

At Telstra Health, the National Cancer Screening Register already supported bowel and cervical cancer screening workflows through a large legacy monolithic platform.

Australia was introducing a new Lung Cancer Screening Program, and the engineering challenge was integrating lung screening participants into the existing ecosystem without tightly coupling new functionality into the legacy monolith.

The new platform was designed using:

- Spring Boot microservices
    
- Kafka-based event-driven architecture
    
- Camunda workflow orchestration
    
- Kubernetes on AWS EKS
    

The system had to:

- ingest participant events
    
- process participant demographics and screening history
    
- evaluate assessments and eligibility
    
- determine next screening actions
    
- create or continue screening pathways
    
- coordinate between legacy and modern distributed systems
    

The complexity came from the fact that healthcare workflows are highly sensitive:

- duplicate workflows could lead to incorrect participant handling
    
- retries could accidentally trigger duplicate actions
    
- eventual consistency between systems had to be carefully managed
    
- participant state existed across multiple services and databases
    
- the system needed to be resilient during async failures and infrastructure instability
    

Additionally, the platform had to support future screening programs and scale independently from the monolith.

---

# T — Task

I was responsible for helping design and implement the event-driven participant orchestration flow between the legacy monolith and the new distributed microservices platform.

My primary focus areas included:

- designing reliable event processing patterns
    
- ensuring idempotent workflow execution
    
- handling retries safely across distributed systems
    
- designing Kafka event contracts
    
- implementing async orchestration using Camunda
    
- supporting scalable participant ingestion and processing
    
- improving resiliency in Kubernetes-based deployments
    
- ensuring downstream actions were not duplicated during retries or failures
    

One of the hardest engineering problems was ensuring idempotent participant workflow creation in an eventually consistent distributed system.

Kafka consumers, Camunda retries, infrastructure restarts, and transient failures could all potentially result in duplicate participant processing or duplicate pathway creation.

In a healthcare screening system, duplicate workflow execution could create serious operational and compliance issues.

---

# A — Action

## Architecture Design

I helped design an event-driven integration model where:

- the legacy monolith published participant-related events
    
- Kafka acted as the durable event backbone
    
- Spring Boot microservices consumed and processed events asynchronously
    
- Camunda orchestrated long-running participant workflows
    
- downstream systems handled assessment evaluation and screening decisions
    

Instead of synchronous REST orchestration between systems, we intentionally adopted asynchronous event-driven communication.

This reduced coupling between systems and improved scalability and resiliency.

---

## Idempotency Design

A major focus area was ensuring that retries were safe.

We designed the processing flow so that participant workflow creation became idempotent.

The implementation strategy included:

- using participant identifiers and workflow correlation identifiers as unique processing keys
    
- validating whether workflows already existed before creating new screening pathways
    
- designing consumers to safely reprocess events without creating duplicate downstream actions
    
- ensuring retry operations became functionally repeatable
    
- preventing duplicate participant state transitions during transient failures
    

This was critical because Kafka and workflow engines operate under at-least-once delivery semantics.

Without proper idempotency handling, retries from:

- Kafka consumers
    
- Camunda workflow retries
    
- Kubernetes pod restarts
    
- network failures
    
- deployment interruptions
    

could all potentially create duplicate actions.

---

## Retry and Failure Handling

The system included multiple async processing stages.

To improve resiliency, we implemented:

- retry handling for transient downstream failures
    
- batching strategies for participant processing
    
- async workflow coordination through Camunda
    
- failure recovery mechanisms for interrupted workflows
    
- observability and logging improvements for distributed tracing
    
- safe reprocessing capabilities
    

We also had to carefully think about where retries should happen.

For example:

- infrastructure-level retries alone were insufficient
    
- workflow-level retries needed to avoid duplicating participant actions
    
- consumer-level retries needed proper validation safeguards
    

This required designing the entire workflow around safe replayability.

---

## Kubernetes and Scalability

The platform was deployed on AWS EKS.

We designed the system so that:

- services could scale independently
    
- event consumers could process workloads asynchronously
    
- participant processing could continue even during transient infrastructure failures
    
- deployments became more isolated compared to the monolithic system
    

Batching strategies were introduced for efficiency during larger participant ingestion scenarios.

---

## Cross-Team Collaboration

The work involved collaboration across:

- legacy platform teams
    
- workflow teams
    
- integration teams
    
- infrastructure teams
    
- downstream consumer systems
    

A major part of the challenge was aligning event contracts and ensuring all systems behaved correctly under retries and async execution.

I contributed to technical discussions around:

- event flow design
    
- retry behavior
    
- workflow boundaries
    
- idempotency handling
    
- async orchestration
    
- failure recovery strategies
    

---

# R — Result

The platform successfully enabled the integration of the Lung Cancer Screening Program into the national screening ecosystem.

The architecture provided:

- scalable async participant processing
    
- resilient event-driven communication
    
- safer retry and failure handling
    
- improved separation from the legacy monolith
    
- reusable integration patterns for future screening workflows
    
- independent scalability through Kubernetes-based microservices
    

The system was significantly more flexible and maintainable compared to extending the legacy synchronous monolith directly.

The event-driven model also improved long-term extensibility for future healthcare screening initiatives.

---

# Strong Technical Angle

## Core Engineering Problem

Ensuring reliable idempotent workflow orchestration in an eventually consistent distributed healthcare system.

---

# Key Architectural Decisions and Reasoning

|Decision|Why We Chose It|Tradeoff|
|---|---|---|
|Kafka-based async architecture|Reduced coupling and improved scalability|Accepted eventual consistency|
|Microservices instead of extending monolith|Independent deployment and future scalability|Increased distributed system complexity|
|Camunda workflow orchestration|Better handling of long-running workflows and retries|Additional operational complexity|
|Idempotent consumers|Safe retries under at-least-once delivery|Additional validation logic|
|Kubernetes on EKS|Independent scaling and resiliency|Infrastructure complexity|
|Async processing instead of synchronous REST chaining|Improved fault tolerance and throughput|Harder observability and debugging|
|Batching participant processing|Improved throughput and operational efficiency|More complex retry handling|
|Event-driven integration|Better future extensibility|Event ordering and consistency challenges|

---

# Important Distributed Systems Concepts Demonstrated

- Event-driven architecture
    
- Eventual consistency
    
- Idempotency
    
- At-least-once delivery semantics
    
- Async orchestration
    
- Distributed retries
    
- Workflow correlation
    
- Failure recovery
    
- Kubernetes resiliency
    
- Long-running workflow orchestration
    
- Service decoupling
    
- Replay-safe processing
    
- Distributed observability
    

---

# Common Follow-Up Questions and Strong Answers

## 1. Why did you choose Kafka instead of synchronous REST communication?

We wanted to reduce coupling between the monolith and the new platform. Synchronous orchestration would tightly couple availability and latency between services. Kafka allowed services to process independently, absorb spikes, retry safely, and scale asynchronously.

---

## 2. Why was idempotency important in your system?

The system used retries extensively across Kafka consumers, workflow execution, and infrastructure recovery. Without idempotency, retries could create duplicate participant workflows or duplicate state transitions, which is unacceptable in healthcare systems.

---

## 3. How did you ensure retries were safe?

We designed the processing logic to be replay-safe. Before creating workflows or downstream actions, we validated whether equivalent workflows already existed using participant and workflow correlation identifiers.

---

## 4. What delivery guarantees did Kafka provide?

Kafka primarily provided at-least-once delivery semantics in our architecture. That meant consumers had to be designed idempotently because duplicate delivery was possible during retries or failures.

---

## 5. Why did you use Camunda?

The participant workflows were long-running and involved multiple async stages and retries. Camunda provided better orchestration, workflow visibility, retry management, and state handling compared to implementing complex workflow coordination manually.

---

## 6. What was the hardest debugging challenge?

One of the hardest parts was tracing async failures across multiple services and workflow stages. Distributed systems make debugging difficult because failures can happen asynchronously and across different infrastructure layers.

---

## 7. How did Kubernetes help your architecture?

Kubernetes allowed independent scaling and resilient deployments. Consumer services could scale horizontally based on workload, and failures became more isolated compared to the monolithic architecture.

---

## 8. What tradeoff did you accept with event-driven architecture?

The biggest tradeoff was accepting eventual consistency instead of immediate consistency. In return, we gained better scalability, resiliency, and decoupling between systems.

---

## 9. How would this system behave at 10x scale?

Kafka partitions and consumer scaling would help distribute workload horizontally. We would also need to carefully monitor workflow bottlenecks, downstream dependencies, and database contention under higher throughput.

---

## 10. Why not simply extend the existing monolith?

The monolith already handled multiple screening programs and was becoming difficult to evolve. Extending it further would increase coupling and deployment risk. The microservice platform provided better scalability and future extensibility.

---

## 11. What would happen if a pod crashed during processing?

The event would be retried because of Kafka’s delivery semantics. Since processing was designed to be idempotent, replaying the event would not create duplicate workflows or invalid participant state transitions.

---

## 12. How did batching improve the system?

Batching improved throughput and reduced overhead during participant ingestion scenarios. However, batching also required careful retry handling because partial failures inside batches can become complex.

---

## 13. What observability improvements did you make?

We improved logging and distributed tracing visibility across async processing stages so workflows could be correlated and failures could be diagnosed more effectively.

---

## 14. What is the biggest lesson you learned from this project?

Distributed systems become significantly harder once retries, async workflows, and eventual consistency are introduced. Reliability must be designed intentionally from the beginning, especially in healthcare systems where correctness matters more than raw throughput.

---

# Staff / SMTS-Level Themes Hidden Inside This Story

## Technical Leadership

- designing integration patterns
    
- driving resiliency decisions
    
- handling distributed systems tradeoffs
    
- contributing to architecture discussions
    

---

## Engineering Maturity

- designing for failure
    
- replay-safe systems
    
- retry-aware architecture
    
- operational thinking
    

---

## System Design Depth

- eventual consistency
    
- async orchestration
    
- distributed workflow coordination
    
- idempotency patterns
    
- event-driven architecture
    

---

# Important Improvements To Make During Verbal Delivery

Do NOT:

- say “we” too much
    
- explain too many technologies without purpose
    
- talk only at project level
    
- sound like a feature developer
    

Instead:

- focus on the hard engineering problem
    
- explain why the problem was difficult
    
- explain tradeoffs
    
- explain failure handling
    
- explain WHY your decisions mattered
    
- explain operational complexity
    

---

# 2-Minute Executive Version

At Telstra Health, I worked on integrating Australia’s new Lung Cancer Screening Program into the National Cancer Screening Register platform.

The challenge was modernizing participant orchestration from a legacy monolithic system into an event-driven microservice architecture using Kafka, Spring Boot, Camunda, and Kubernetes on AWS EKS.

One of the hardest engineering problems was ensuring idempotent workflow execution in a distributed async system. Because Kafka consumers, workflow retries, and infrastructure restarts could all trigger duplicate processing, we had to design the entire workflow to be replay-safe.

I contributed to designing the event orchestration flow, idempotent processing strategies, retry handling, and async workflow coordination between the monolith and distributed services.

We intentionally chose an event-driven architecture to improve scalability, resiliency, and long-term extensibility, while accepting eventual consistency as a tradeoff.

The result was a more scalable and maintainable platform that successfully enabled the new Lung Cancer Screening Program while providing reusable integration patterns for future screening workflows.

---

# Final Interview Advice

This story is strongest when you position yourself as:

- a distributed systems engineer
    
- a reliability-focused backend engineer
    
- someone who designs for failure
    
- someone who understands tradeoffs deeply
    
- someone who can modernize legacy systems safely
    

NOT merely as:

- a Spring Boot developer
    
- a Kafka developer
    
- a feature implementer