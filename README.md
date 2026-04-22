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

  Ensures continuous data consistency across systems.

* **On-Demand Manual Overrides:**  
  Developed secure manual sync triggers with **logic-based guards**, allowing controlled execution of sync operations while preventing conflicts with scheduled jobs.

---

## 📊 2. High-Fidelity Document Generation (Excel & PDF)

Implemented backend-driven reporting pipelines for generating audit-ready documents with strict formatting and business logic.

* **Scientific Notation Mitigation:**  
  Applied strict Excel formatting (`numFmt: '@'`) to prevent large numeric values (e.g., 16-digit barcodes) from converting into scientific notation.

* **Intelligent Threshold Monitoring Logic:**  
  Integrated business rules directly into report generation:
  - 🟡 **Critical Priority:** Items expiring within 30 days (Yellow: `#ffff99`)
  - 🔵 **High Priority:** Items expiring within 6 months (Light Blue: `#94dcf8`)
  - 🔴 **Variance Detection:** Quantity mismatches flagged in Red (`#ff5050`)

  This transforms raw reports into actionable insights.

---

## ✉️ 3. High-Fidelity Communication Engine (Outlook-Proof)

Developed a decoupled email service optimized for compatibility with restrictive email clients like Microsoft Outlook.

* **Outlook Rendering Mitigation:**  
  Built a table-based HTML structure using inline CSS and `bgcolor` attributes to enforce consistent rendering across email clients.

* **CID-Based Media Embedding:**  
  Replaced Base64 images with **CID attachments** to ensure:
  - Faster loading
  - Better security compliance
  - No blocking by email clients

* **Brand-Consistent Layout:**  
  Maintained consistent design using:
  - Header accents (`#ED017F`)
  - Structured typography
  - Clear data hierarchy

---

## 🔔 4. Real-Time Event Management Ecosystem

Designed a scalable notification system supporting real-time alerts and persistent state tracking.

* **Persistent Notification States:**  
  Extended the Prisma schema to support **read/unread states**, ensuring synchronization across sessions and devices.

* **Dynamic Message Construction:**  
  Implemented logic-driven messaging based on:
  - Location ID
  - Category configuration
  - Severity level  

  This ensures context-aware and meaningful notifications.

* **Asynchronous Processing:**  
  Decoupled notification dispatch logic from core transactions using async patterns, maintaining **fast database write performance**.

---

## ⚙️ 5. Advanced Business Logic & Compliance

* **Dual-Threshold Chronological Processing:**  
  Scheduled daily background jobs (08:00 AM server time) to evaluate records against time-based thresholds.

* **Heuristic GS1-128 Parsing Engine:**  
  Built a regex-driven parser to accurately extract:
  - Barcode
  - Lot Number
  - Expiry Date  

  from complex GS1-128 encoded strings.

* **Resilient Connection Management:**  
  Implemented retry and failover mechanisms to ensure:
  - Safe query execution
  - Proper connection release
  - Stability during unexpected failures

---

## 💡 The Result

Delivered an enterprise-grade backend system that ensures:
- High concurrency performance  
- Reliable legacy-to-cloud synchronization  
- Real-time event handling  
- 100% data integrity and consistency  

---

## 👥 Contributors
- **Lead Backend Developer**: Sir M Saleem  
- **Technical Management**: Sir Ghayoor Haider  
