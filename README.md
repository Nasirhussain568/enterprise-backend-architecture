# Enterprise Backend Architecture: Case Study in Legacy-Cloud Interoperability 🏗️

This repository documents the architecture and implementation of a high-scale enterprise backend designed to bridge the gap between legacy on-premise infrastructure and modern cloud-native systems.

## 🚀 Executive Summary
I architected and engineered a resilient server-side solution addressing the critical challenges of **Legacy Data Modernization** (Legacy SQL to Cloud integration), **Automated Enterprise Reporting** (Excel/PDF generation), and **Real-Time Event Management**.

The primary objective was to transform disconnected, raw data streams into actionable business intelligence while ensuring **absolute data integrity** within a high-stakes enterprise environment.

---

## 🛠️ Tech Stack Depth
![TypeScript](https://img.shields.io/badge/TypeScript-007ACC?style=for-the-badge&logo=typescript&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)
![NestJS](https://img.shields.io/badge/NestJS-E0234E?style=for-the-badge&logo=nestjs&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-4EA94B?style=for-the-badge&logo=mongodb&logoColor=white)
![Prisma](https://img.shields.io/badge/Prisma-2D3748?style=for-the-badge&logo=Prisma&logoColor=white)
![Legacy SQL](https://img.shields.io/badge/SQL-FF0000?style=for-the-badge&logo=firebird&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)

---

## 🏛️ 1. Hybrid Data Engineering (The "Sync Engine")

Designed and implemented a high-throughput synchronization layer to orchestrate data flow between legacy on-site systems and modern cloud backends.

* **High-Concurrency Connection Pooling:**  
  Engineered a robust SQL connection pool to eliminate race conditions and connection bottlenecks.  
  This enables **10+ concurrent database transactions**, allowing background sync processes and high-frequency API requests to run simultaneously without performance degradation.

* **Bidirectional Deletion Reconciliation:**  
  Implemented a **'Last-Seen Timestamp Algorithm'** to resolve hard-deletion inconsistencies in legacy systems.  
  This logic tracks record visibility across sync cycles and safely removes orphaned ("ghost") records from the cloud database.

* **Dual-Database Cron Synchronization:**  
  Built automated **cron jobs** to periodically:
  - Extract data from legacy SQL
  - Transform it into normalized format
  - Sync it into the cloud database  

* **On-Demand Manual Overrides:**  
  Developed secure manual sync triggers with **logic-based guards**, allowing controlled execution of sync operations while preventing conflicts with scheduled jobs.

---

## 📊 2. High-Fidelity Document Generation (Excel & PDF)

Implemented backend-driven reporting pipelines for generating audit-ready documents with strict formatting and business logic.

* **Scientific Notation Mitigation:**  
  Applied strict Excel formatting (`numFmt: '@'`) to prevent large numeric values (e.g., 16-digit clinical barcodes) from converting into scientific notation.

* **Intelligent Threshold Monitoring Logic:**  
  Integrated business rules directly into report generation:
  - 🟡 **Critical Priority:** Items expiring within 30 days (Yellow: `#ffff99`)
  - 🔵 **High Priority:** Items expiring within 6 months (Light Blue: `#94dcf8`)
  - 🔴 **Variance Detection:** Quantity mismatches flagged in Red (`#ff5050`)

---

## ✉️ 3. High-Fidelity Communication Engine (Outlook-Proof)

Developed a decoupled email service optimized for compatibility with restrictive email clients like Microsoft Outlook.

* **Outlook Rendering Mitigation:**  
  Built a table-based HTML structure using inline CSS and `bgcolor` attributes to enforce consistent rendering and **100% white backgrounds** across desktop clients.

* **CID-Based Media Embedding:**  
  Replaced Base64 images with **CID attachments** to ensure faster loading and prevent security filters from blocking logos as "unsafe."

* **Brand-Consistent Layout:**  
  Maintained consistent design using header accents (`#ED017F`) and structured data hierarchy.

---

## 🔔 4. Real-Time Event Management Ecosystem

Designed a scalable notification system supporting real-time alerts and persistent state tracking.

* **Persistent Notification States:**  
  Extended the data schema to support **read/unread states**, ensuring synchronization across sessions and devices.

* **Dynamic Message Construction:**  
  Implemented logic-driven messaging based on Location ID and severity level to ensure context-aware notifications.

* **Asynchronous Processing:**  
  Decoupled notification dispatch logic from core transactions using async patterns, maintaining fast database write performance.

---

## 🛡️ 5. Relational Integrity & Schema Evolution (New)

Architected complex relational logic to handle entity lifecycles while maintaining strict database constraints.

* **Optional Relation Strategy:**  
  Evolved the schema to use **Optional Relations (`Int?`)** for critical modules (e.g., Departments). This allows for a robust **Soft-Delete workflow**, ensuring that deleting a user or department does not trigger catastrophic cascading failures across the system.
* **Deletion Constraint Resolution:**  
  Resolved a critical "Blocker" issue where core entities (e.g., Lab Setups) could not be deleted if they were referenced by user permissions. Engineered a pre-deletion cleanup protocol that programmatically unlinks child references before the parent deletion, ensuring a 100% success rate for administrative cleanup tasks.
* **Advanced Error Handling:**  
  Implemented targeted handling for **Foreign Key Violations (P2003)**. Instead of system crashes, the API now provides intelligent `ConflictException` and `NotFoundException` feedback to the user.

---

## ⚙️ 6. Advanced Business Logic & Compliance

* **Dual-Threshold Chronological Processing:**  
  Scheduled daily background jobs (08:00 AM server time) to evaluate records against time-based thresholds.
* **Heuristic GS1-128 Parsing Engine:**  
  Built a regex-driven parser to accurately extract Barcodes, Lot Numbers, and Expiry Dates from complex encoded strings.
* **Resilient Connection Management:**  
  Implemented retry and failover mechanisms to ensure safe connection releases and stability during heavy SQL query execution.

---

## 💡 The Result

Delivered an enterprise-grade backend system that ensures:
- High concurrency performance  
- Reliable legacy-to-cloud synchronization  
- Seamless relational management and entity deletion
- 100% data integrity and consistency  

---

## 👥 Contributors
- **Lead Backend Developer**: Sir M Saleem  
- **Technical Management**: Sir Ghayoor Haider
