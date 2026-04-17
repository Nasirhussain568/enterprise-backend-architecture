# Enterprise Backend Architecture: Case Study in Legacy-Cloud Interoperability 🏗️

This repository documents the architecture and implementation of a high-scale enterprise backend designed to bridge the gap between legacy on-premise infrastructure and modern cloud-native systems.

## 🚀 Executive Summary
I architected and engineered a resilient server-side solution addressing the critical challenges of **Legacy Data Modernization** (Legacy SQL to Cloud integration), **Automated Enterprise Reporting** (Excel/PDF generation), and **Real-Time Event Management**. 

The primary objective was to transform disconnected, raw data streams into actionable business intelligence, ensuring **absolute data integrity** within a high-stakes enterprise environment.

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
Designed a high-throughput synchronization layer to orchestrate data flow between legacy on-site systems and modern cloud backends.

*   **High-Concurrency Connection Pooling:** Eliminated race conditions and connection bottlenecks by engineering a robust SQL Connection Pool. This architecture facilitates **10+ concurrent database transactions**, allowing heavy background synchronization and high-frequency API requests to operate simultaneously.
*   **Bidirectional Deletion Reconciliation:** Implemented a 'Last-Seen' timestamp algorithm to resolve hard-deletion discrepancies in legacy systems. This mechanism accurately identifies and purges orphaned "ghost" records from the primary cloud database.
*   **Dual-Database Cron Synchronization:** Established automated **Cron Jobs** to periodically ingest, transform, and sync legacy data directly into the cloud-native storage layer.
*   **On-Demand Manual Overrides:** Engineered secure manual synchronization triggers utilizing logic gates to differentiate between scheduled maintenance and user-initiated state updates.

## 📊 2. High-Fidelity Document Generation (Excel & PDF)
Reporting demands uncompromising precision. I utilized specialized backend libraries to programmatically generate complex, audit-ready digital documents.

*   **Scientific Notation Mitigation:** Engineered strict formatting protocols (`numFmt: '@'`) to prevent spreadsheet software from truncating 16-digit clinical barcodes into scientific notation (e.g., `1.08E+14`).
*   **Intelligent Threshold Monitoring:**
    *   🟡 **Critical Priority:** Automated Yellow (#ffff99) highlighting for items expiring within 30 days.
    *   🔵 **High Priority:** Automated Light Blue (#94dcf8) highlighting for items expiring within 6 months.
    *   🔴 **Variance Logic:** Visual Red (#ff5050) flagging for critical quantity discrepancies.

## ✉️ 3. High-Fidelity Communication Engine (Outlook-Proof)
Developed a decoupled notification service meticulously engineered to bypass the rendering constraints of legacy mail clients like Microsoft Outlook.

*   **Outlook Rendering Mitigation:** Designed a table-based HTML architecture utilizing inline CSS and explicit `bgcolor` declarations. This preserves **100% white backgrounds (#ffffff)** against Outlook’s aggressive shading engine.
*   **CID Media Embedding:** Replaced Base64 strings with **CID (Content-ID) attachments** to ensure logos load instantly and safely without being blocked as "unsafe" by desktop security filters.
*   **Brand-Consistent Typography:** Enforced strict visual hierarchy using **Brand Header Accents (#ED017F)** and bolded metadata labels.

## 🔔 4. Real-Time Event Management Ecosystem
Engineered a unified, multi-channel notification framework for critical system alerts and state changes.

*   **Schema Persistent States:** Updated the **Prisma Data Schema** to include persistent unread/read states, ensuring that alert statuses remain synchronized across multiple user sessions and devices.
*   **Logic-Driven Messaging:** Enhanced the service layer to dynamically construct context-aware messages based on specific location IDs, category setups, and record severity levels.
*   **Asynchronous Triggering:** Optimized system performance by decoupling heavy dispatch logic from core business transactions, ensuring sub-millisecond database write speeds.

## ⚙️ 5. Advanced Business Logic & Compliance
*   **Dual-Threshold Chronological Vectors:** Configured daily background processes executing at 08:00 AM server-time to scan global datasets against real-time chronological vectors.
*   **Heuristic Parsing:** Developed a robust regex-driven parser to accurately extract Barcodes, Lot Numbers, and Expiration Dates from composite GS1-128 strings.
*   **Recursive Connection Management:** Engineered an auto-retry and failover mechanism within the SQL service, guaranteeing that pooled connections are safely released even during unhandled query exceptions.

---

## 💡 The Result
By fusing a **High-Concurrency Connection Pooling** architecture with a **Real-Time Notification Ecosystem**, I delivered an enterprise-grade platform that guarantees 100% data parity between legacy on-premise hardware and modern cloud infrastructure.

---

## 👥 Contributors
- **Lead Backend Developer**: Sir M Saleem
- **Technical Management**: Sir Ghayoor Haider
