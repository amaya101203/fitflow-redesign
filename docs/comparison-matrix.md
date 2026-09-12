Activity 3: Technology Comparison Matrix

To make the final stack selection auditable, each layer's leading candidates were scored 1–5 against weighted criteria reflecting FitFlow's priorities: Security/Compliance and Performance were weighted highest given the health-data context and real-time UX demands; Cost and AI/ML support were weighted moderately; Development Speed and Maintainability were weighted to reflect the mid-sized team's capacity.

Criteria weights used across all matrices

Criterion	Weight

Performance	20%

Security / Compliance	20%

Development Speed	15%

Scalability	15%

Maintainability	10%

AI/ML Support	10%

Cost	10%



3.1 Frontend Weighted Matrix

Option	Performance (20%)	Security (20%)	Dev Speed (15%)	Scalability (15%)	Maintain. (10%)	AI/ML (10%)	Cost (10%)	Weighted Total

Flutter	4	4	4	4	4	4	4	4.00

React Native	3	4	5	4	4	3	4	3.85

Kotlin Multiplatform	5	4	3	4	3	3	3	3.75

Swift/SwiftUI	5	5	3	2	2	4	2	3.55



3.2 Backend Weighted Matrix

Option	Performance (20%)	Security (20%)	Dev Speed (15%)	Scalability (15%)	Maintain. (10%)	AI/ML (10%)	Cost (10%)	Weighted Total

Node.js/NestJS	4	4	5	4	5	3	4	4.15

Python/FastAPI	4	4	4	4	4	5	4	4.10

Go	5	4	3	5	3	2	5	4.00



3.3 Database Weighted Matrix

Option	Performance (20%)	Security (20%)	Dev Speed (15%)	Scalability (15%)	Maintain. (10%)	AI/ML (10%)	Cost (10%)	Weighted Total

PostgreSQL	4	5	4	4	5	3	4	4.20

MongoDB	4	3	4	4	4	3	3	3.60

Firebase (Firestore/RTDB)	4	3	5	5	3	3	3	3.80

DynamoDB	5	4	3	5	3	2	4	3.90



3.4 Authentication Weighted Matrix

Option	Performance (20%)	Security (20%)	Dev Speed (15%)	Scalability (15%)	Maintain. (10%)	AI/ML (10%)	Cost (10%)	Weighted Total

Firebase Auth	4	3	5	5	4	2	5	4.00

AWS Cognito	4	4	3	5	3	2	4	3.70

Auth0	4	5	4	5	4	2	3	4.05

Supabase Auth	3	3	4	3	4	2	4	3.25



3.5 Consolidated Recommendation

Highlighted (green) rows indicate the highest-scoring option per layer. The weighted matrix confirms the choices justified qualitatively in Activities 1 and 2:

•	Frontend: Flutter (highest weighted total, driven by performance and web/mobile reusability).

•	Backend: Node.js/NestJS (highest weighted total, driven by development speed and maintainability, narrowly ahead of FastAPI).

•	Database: PostgreSQL (highest weighted total once security/compliance is weighted at 20%, ahead of Firebase's raw scalability score).

•	Authentication: Auth0 (highest weighted total, driven by its security/compliance strength for a health-data product).

Recommended full stack: Flutter (frontend, mobile + web) → NestJS (backend API) → PostgreSQL + Redis + Firebase Realtime DB (data layer) → Auth0 (identity) → TensorFlow / TensorFlow Lite (AI layer), deployed on a managed Kubernetes or serverless container platform (e.g., AWS ECS/EKS or Google Cloud Run) for elastic scaling.

 



