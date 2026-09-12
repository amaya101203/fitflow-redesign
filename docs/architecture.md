Activity 4: High-Level Architecture Design

The architecture below implements the recommended stack from Activity 3 as a set of loosely coupled services behind an API Gateway, allowing the AI, nutrition (computer vision), and social components to scale independently based on their very different load profiles.

4.1 Key Components

•	Client layer: Flutter mobile apps (iOS/Android) and Flutter Web dashboard, plus optional wearable/HealthKit sync.

•	API Gateway: single entry point handling JWT validation (via Auth0), rate limiting, and request routing to backend microservices.

•	Backend microservices (NestJS): Auth Service, Workout \& Planning Service, Social Service, Nutrition Service, and Notification Service — each independently deployable and scalable.

•	AI microservice: hosts the workout-recommendation model (TensorFlow) for cloud inference, complementing on-device TensorFlow Lite personalization for offline/low-latency use.

•	Computer vision service: dedicated service for food-image recognition, isolated from the main API so heavy image-processing load cannot degrade core app responsiveness.

•	Event bus (Kafka/Pub-Sub): decouples the Social Service from the Notification Service, so a spike in social activity does not block push notification delivery.

•	Data layer: PostgreSQL for transactional/relational data, Redis for caching and session state, Firebase Realtime Database for low-latency social feed sync, S3-compatible object storage for food/progress images, and a BigQuery-style analytics warehouse fed asynchronously for reporting.

4.2 Critical Data Flows

Personalized workout plan generation

Mobile app → API Gateway → Workout \& Planning Service → AI Microservice (cloud model, or on-device TFLite when offline) → result persisted to PostgreSQL → response streamed back to the app, with the plan cached in Redis for fast re-access.

Social sharing / community challenges

Mobile app → API Gateway → Social Service → write to Firebase Realtime Database (fan-out to followers' feeds) → event published to the Event Bus → Notification Service consumes the event and pushes alerts to relevant users' devices.

Nutrition tracking (camera-based logging)

Mobile app captures a food photo → image uploaded to Object Storage → Nutrition Service invokes the Computer Vision Service to identify food items and estimate nutrition values → confirmed log written to PostgreSQL → dashboard updated in the app via the API Gateway.

4.3 Security, Scalability \& Integration Considerations

•	Security: TLS everywhere; JWT-based auth validated at the gateway; field-level encryption for health-sensitive columns in PostgreSQL; anonymization pipeline before any data reaches the analytics warehouse or is shared with healthcare-provider partners.

•	Scalability: stateless microservices behind the gateway auto-scale horizontally (Kubernetes HPA or serverless concurrency); Redis absorbs read-heavy spikes; the Event Bus smooths bursty social traffic so downstream services are not overwhelmed.

•	Integration: third-party gym partner APIs and HealthKit/Health Connect sync integrate through dedicated adapters behind the Workout Service, keeping external-API volatility isolated from core services.

•	Compliance: data residency and processing agreements (GDPR, and HIPAA-equivalent BAAs where applicable) are enforced at the infrastructure layer, with audit logging on all services that touch personally identifiable or health data.

&#x20;

Figure 4.1 — FitFlow high-level system architecture

4.4 Architecture Decision Record (ADR)

Field	Details

ADR ID / Title	ADR-001: Selection of Core Technology Stack for the FitFlow Redesign

Status	Accepted

Date	12 September 2026

Context	FitFlow's retention had declined sharply, and user research identified a personalization gap, social isolation, and high-friction nutrition logging as root causes. The redesign requires a frontend that delivers a consistent, high-performance iOS/Android/web experience; a backend that can serve both transactional health data and real-time social features; and an architecture that satisfies GDPR and health-data-adjacent compliance while remaining affordable for a mid-sized team.

Decision	Adopt Flutter for the client layer (mobile + web), NestJS (Node.js) for backend microservices, PostgreSQL as the primary relational store with Redis for caching and Firebase Realtime Database for the social feed, Auth0 for authentication/authorization, and TensorFlow/TensorFlow Lite for cloud and on-device AI personalization, deployed on a managed container platform (Kubernetes or serverless containers) behind an API Gateway.

Alternatives Considered	React Native (rejected as primary due to weaker web parity and animation-performance headroom, though viable for a future admin console); Kotlin Multiplatform and Swift/SwiftUI (rejected as primary due to weaker cross-platform coverage); FastAPI/Python backend (rejected as primary due to lower team familiarity, though retained as a candidate host for standalone ML services); MongoDB/DynamoDB (rejected as the primary store due to weaker fit for relational health data, though Firebase is retained specifically for real-time social sync).

Consequences (Positive)	Single Flutter codebase reduces long-term maintenance cost across three platforms; NestJS's modular structure supports a growing mid-sized team; PostgreSQL + Auth0 give strong compliance posture; independent microservices allow the AI and computer-vision workloads to scale without affecting core API latency.

Consequences (Negative / Risks)	Dart introduces a new language for a JavaScript-leaning team, requiring initial training investment; running both PostgreSQL and Firebase adds operational complexity (two data stores to secure and monitor); Auth0 costs scale with monthly active users and should be re-evaluated if user growth significantly exceeds projections.

 



