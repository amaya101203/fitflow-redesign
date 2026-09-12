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



