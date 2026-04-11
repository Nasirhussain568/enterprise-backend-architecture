# Enterprise Backend Architecture: Case Study in Legacy-Cloud Interoperability 🏗️

This repository documents the architecture and implementation of a high-scale enterprise platform designed to bridge the gap between legacy on-premise infrastructure and modern cloud-native systems.

## 🚀 Executive Summary
I architected and engineered a resilient backend solution addressing the critical challenges of **Legacy Data Modernization** (Firebird SQL to Cloud integration) and **Automated Enterprise Reporting** (Excel/PDF generation & MJML communication). 

The primary objective was to transform disconnected, raw data streams into actionable, high-fidelity business intelligence, ensuring **absolute data integrity** within a life-critical medical environment.

---

## 🛠️ Tech Stack Depth
![TypeScript](https://img.shields.io/badge/TypeScript-007ACC?style=for-the-badge&logo=typescript&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)
![NestJS](https://img.shields.io/badge/NestJS-E0234E?style=for-the-badge&logo=nestjs&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-4EA94B?style=for-the-badge&logo=mongodb&logoColor=white)
![Prisma](https://img.shields.io/badge/Prisma-2D3748?style=for-the-badge&logo=Prisma&logoColor=white)
![Firebird](https://img.shields.io/badge/Firebird-FF0000?style=for-the-badge&logo=Firebird&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Redux](https://img.shields.io/badge/Redux-764ABC?style=for-the-badge&logo=redux&logoColor=white)
![Ant Design](https://img.shields.io/badge/Ant%20Design-0170FE?style=for-the-badge&logo=antdesign&logoColor=white)

---

## 🏛️ 1. Hybrid Data Engineering (The "Sync Engine")
Designed and implemented a high-throughput synchronization layer to seamlessly orchestrate data flow between legacy systems and modern cloud backends.

*   **High-Concurrency Connection Pooling:** Eliminated race conditions and connection bottlenecks (`lazy_count` errors) by engineering a robust Firebird Connection Pool. This architecture facilitates 10+ concurrent database transactions, allowing heavy background synchronization and high-frequency frontend requests to operate simultaneously on independent threads.
*   **Bidirectional Deletion Reconciliation:** Implemented a 'Last-Seen' timestamp algorithm to resolve hard-deletion discrepancies in legacy systems. This mechanism accurately identifies and purges orphaned "ghost" records from the local database that no longer exist in the source Firebird instance.
*   **Dual-Database Cron Synchronization:** Established a highly available Primary-Secondary database topology. Deployed automated **Cron Jobs** to periodically ingest, transform, and sync legacy data from the on-premise **Firebird SQL (Secondary)** directly into **MongoDB/PostgreSQL (Primary)**.
*   **On-Demand Manual Overrides:** Engineered secure, high-priority manual synchronization triggers, utilizing logic gates (`isManually`) to differentiate between scheduled background maintenance and user-initiated state updates.
*   **Sync State Persistent Monitoring:** Implemented a **Singleton Metadata Model** (`LastSync`) via a fixed-ID document pattern in MongoDB, providing the frontend with an optimized, lightweight heartbeat for synchronization status.

## 📊 2. High-Fidelity Document Generation (Excel & PDF)
Medical compliance reporting demands uncompromising precision. I utilized **ExcelJS** to programmatically generate complex, audit-ready digital documents.

*   **Scientific Notation Mitigation:** Engineered strict string-formatting protocols (`numFmt: '@'`) to prevent Excel from inadvertently truncating or corrupting 16-digit clinical barcodes into scientific notation (e.g., `1.08E+14`).
*   **Cell-Merging & Layout Logic:** Architected complex row-spanning algorithms to accommodate variable-length clinical comments and hierarchical Sales Rep data across an expansive 18-column (A-R) grid.
*   **Intelligent Expiry Monitoring:**
    *   🟡 **Gate 1 (Danger):** Automated Yellow (#ffff99) highlighting for items expiring within 30 days.
    *   🔵 **Gate 2 (Warning):** Automated Light Blue (#94dcf8) highlighting for items expiring within 6 months.
    *   🔴 **Variance Logic:** Visual Red (#ff5050) flagging for critical stock quantity discrepancies.

## ✉️ 3. High-Fidelity Communication Engine (Outlook-Proof Architecture)
Developed a decoupled, enterprise-grade notification service meticulously engineered to bypass the rendering constraints of legacy mail clients like Microsoft Outlook.

*   **Outlook "Gray-Scale" Mitigation:** Designed a custom, table-based HTML architecture utilizing inline CSS and explicit `bgcolor` declarations. This guarantees cross-client rendering fidelity and preserves **100% white backgrounds (#ffffff)** against Outlook’s aggressive Dark Mode inversion.
*   **Dynamic Template Consolidation:** Refactored a fragmented template structure into a unified, high-performance TypeScript **Dynamic Template Engine**. Leveraging a "Detail-Array" pattern, the system dynamically injects variable data payloads without altering foundational HTML.
*   **Brand-Consistent Typography:** Enforced strict visual hierarchy and brand consistency using **Pink Header Accents (#ED017F)** and bolded metadata labels, ensuring a seamless user experience from the web dashboard to the email inbox.

## 🎨 4. Frontend Design Engineering & UI/UX Optimization
Resolved complex UI architecture challenges to deliver a fluid, cross-platform experience for clinical personnel.

*   **Live Reactive Tables (Redux):** Engineered real-time state listeners using Redux for Variance Reports. This implementation enables automatic UI hydration and stock recalculation post-edit, entirely deprecating the need for manual page refreshes.
*   **Omni-Stock Navigation Fix:** Eliminated critical navigation deadlocks by decoupling component loading states from warehouse-ID validation, allowing tables to seamlessly fetch and render new data during context switching.
*   **Column Standardization:** Implemented a fluid, percentage-based CSS Grid architecture across the Dashboard. Recalibrated column widths (Stock Code: 12%, Expiry: 10%, Status: 15%) to ensure pixel-perfect consistency across diverse device viewports.
*   **Ant-Design Table Realignment:** Corrected fixed-header misalignment anomalies in Ant-Design tables by enforcing strict percentage constraints and applying `tableLayout="fixed"`, ensuring perfect vertical alignment between headers and body data.

## ⚙️ 5. Advanced Business Logic & Performance
*   **Recursive Connection Management:** Engineered an auto-retry and failover mechanism within the Firebird service, guaranteeing that pooled connections are safely released (`db.detach()`) even during unhandled query exceptions.
*   **Asynchronous Notification Triggers:** Optimized transaction throughput by decoupling email dispatch logic **outside of the Prisma Database Transactions**. This asynchronous execution eliminates transaction timeouts and ensures sub-millisecond database writes.
*   **GS1-128 Parsing:** Developed a robust regex-driven heuristic parser to accurately extract Barcodes, Lot Numbers, and Expiration Dates from composite GS1-128 medical strings.
*   **RBAC & Security:** Enforced strict clinical accountability by integrating JWT-Passport authentication, cryptographically linking every inventory mutation to verified personnel.

## ⏱️ 6. Automated Risk Prediction & Compliance (Expiry Alerts)
Engineered a proactive, automated inventory monitoring protocol that dispatches priority-coded alerts for medical supplies approaching expiration.

*   **Dual-Threshold Architecture:**
    *   🔵 **180-Day Blue Alert:** Triggers exactly 6 months prior to expiry, facilitating inventory rotation planning.
    *   🟡 **30-Day Yellow Alert:** Triggers exactly 1 month prior to expiry, indicating urgent consumption or disposal requirements.
*   **Automated Background Processing:** Configured robust daily Cron jobs (`@nestjs/schedule`) executing at 08:00 AM server-time to scan the global MongoDB dataset against real-time chronological vectors.
*   **Dynamic Routing & Filtering:** Architected a dynamic routing resolution algorithm that maps active `LabSetup` configurations to specific Warehouse IDs, ensuring compliance alerts are instantly routed to the appropriate regional administrators.
*   **Decoupled Service Layer:** Isolated business logic within the `StockExpiryNotificationService` to independently process chronological matrices, isolate inventory targets, and construct payloads for the dispatch module.

---

## 💡 The Result
By fusing a **High-Concurrency Connection Pooling** architecture with a resilient reporting layer and **Live Reactive Redux Tables**, I delivered an enterprise-grade platform that guarantees 100% data parity between legacy on-premise hardware and modern cloud infrastructure. The resulting system transcends basic inventory management to actively predict supply chain risks and automate end-to-end clinical reporting.
