# Architecture_Diagram.png
# <img width="1376" height="768" alt="architecture-diagram" src="https://github.com/user-attachments/assets/70bde7d5-30d3-4d27-a3e2-33ea626298e3" />

**Client interfaces** — the web portal, mobile app, patient self-service portal, and third-party integrations (labs, payers, HIE) all funnel requests downward through a single secure entry point.

**API gateway & security perimeter** — every inbound request passes through authentication (OAuth 2, MFA, RBAC), rate limiting, TLS-terminating routing, and immutable audit logging before touching any business logic.

**Core application services** — the eight domain services (patient records, appointments, clinical docs, billing, notifications, prescriptions, interoperability, and reporting) communicate internally via a PHI-scoped async event bus to decouple workloads.

**Data persistence** — a split storage strategy: an encrypted relational database for structured PHI/PII, a cache layer for sessions and hot reads, a document store for images and attachments, and geo-redundant backups for disaster recovery.

**Security & compliance** — the foundational layer enforcing AES-256/TLS 1.3 encryption, least-privilege IAM, HIPAA/HITECH controls, and continuous vulnerability monitoring via SIEM and pen testing. A zero-trust perimeter and BAA enforcement span the entire stack.

The dashed arrows at the bottom show the bidirectional **FHIR/HL7 data flows** between Vault and external systems like labs and health information exchanges.

