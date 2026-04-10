# Enterprise Backend Architecture: Case Study in Legacy-Cloud Interoperability 🏗️

This repository documents the architecture and implementation of a high-scale enterprise platform designed to bridge the gap between legacy on-premise infrastructure and modern cloud-native systems.

## 🚀 Executive Summary
I engineered a robust backend solution to solve the critical challenges of **Legacy Data Modernization** (Firebird SQL to Cloud) and **Automated Enterprise Reporting** (Excel/PDF engines & MJML communication). 

The primary objective was to transform disconnected, raw data into actionable, high-fidelity business intelligence while ensuring **100% data integrity** in a life-critical medical environment.

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
I engineered a high-performance synchronization layer to manage the data flow between legacy systems and modern backends.

*   **High-Concurrency Connection Pooling:** Resolved critical "Race Condition" crashes (`lazy_count` errors) by architecting a Firebird Connection Pool. This allows the system to handle 10+ simultaneous database requests, enabling heavy background syncs and high-frequency frontend scans to operate on independent threads.
*   **Bidirectional Deletion Reconciliation:** Engineered a "Last-Seen" timestamp algorithm to solve the challenge of hard-deleted records in legacy systems. The engine identifies and removes "ghost" records from the local database that no longer exist in the source Firebird instance during each sync cycle.
*   **Dual-Database Cron Synchronization:** Architected a resilient Primary-Secondary database relationship. Implemented automated **Cron Jobs** to periodically ingest, transform, and sync legacy data from the on-premise **Firebird SQL (Secondary)** database directly into **MongoDB/Postgres (Primary)**.
*   **On-Demand Manual Overrides:** Developed a high-priority manual trigger allowing users to initiate a sync outside the standard scheduler. Engineered logic-gate parameters (`isManually`) to distinguish between background maintenance and user-initiated updates.
*   **Sync State Persistent Monitoring:** Designed a **Singleton Metadata Model** (`LastSync`) using a fixed-ID document pattern in MongoDB. This provides the frontend with a lightweight reference to display the "Last Synced" heartbeat.

## 📊 2. High-Fidelity Document Generation (Excel & PDF)
Reporting in medical environments requires extreme precision. I leveraged **ExcelJS** to recreate complex physical paper trails digitally.

*   **Scientific Notation Fix:** Implemented string-forcing logic (`numFmt: '@'`) to prevent Excel from corrupting 16-digit barcodes into scientific notation (e.g., `1.08E+14`).
*   **Cell-Merging Logic:** Solved complex row-spanning challenges for clinical comments and Sales Rep headers across an 18-column (A-R) grid.
*   **Intelligent Expiry Monitoring:**
    *   🟡 **Gate 1 (Danger):** Automatic Yellow (#ffff99) for items expiring within 1 month.
    *   🔵 **Gate 2 (Warning):** Automatic Light Blue (#94dcf8) for items expiring within 6 months.
    *   🔴 **Variance Logic:** Visual Red (#ff5050) flagging for stock quantity mismatches.

## ✉️ 3. High-Fidelity Communication Engine (Outlook-Proof Architecture)
Built a decoupled, professional notification system engineered specifically to overcome the rendering limitations of enterprise mail clients like Microsoft Outlook.

*   **Outlook "Gray-Scale" Mitigation:** Engineered a custom table-based HTML architecture using inline CSS and explicit `bgcolor` attributes. This ensures that backgrounds stay **100% white (#ffffff)** even when viewed in Outlook’s aggressive Dark Mode.
*   **Dynamic Template Consolidation:** Refactored a high-complexity architecture into a single, high-performance TypeScript-based **Dynamic Template Engine**. This system uses a flexible "Detail-Array" pattern to inject custom fields without modifying underlying HTML.
*   **Brand-Consistent Typography:** Implemented a specific visual hierarchy using **Pink Header Accents (#ED017F)** and bolded metadata labels to match the platform's clinical UI.

## 🎨 4. Frontend Design Engineering & UI/UX Optimization
I solved critical UI challenges to ensure a seamless experience for clinical staff across different operating systems.

*   **Live Reactive Tables (Redux):** Engineered a real-time update listener for Variance Reports. By adding Redux state monitoring to the dashboard, the UI now automatically refreshes and recalculates stock levels after any edit, eliminating the need for manual browser refreshes.
*   **Omni-Stock Navigation Fix:** Resolved a critical navigation bug where the stock table would get "stuck" in a loading state. By decoupling the loading state from the warehouse-ID check, the table now seamlessly fetches new data when switching between registers.
*   **Column Standardization:** Implemented a percentage-based responsive grid across the Dashboard and Stock Management modules. Recalibrated column widths (Stock Code: 12%, Expiry: 10%, Status: 15%) to ensure a pixel-perfect, consistent layout across all screen resolutions.
*   **Ant-Design Table Realignment:** Fixed the "Separate-Table Misalignment" in fixed-header tables. By recalibrating percentage-based widths to 100% and applying `tableLayout="fixed"`, I ensured the header and body columns remain perfectly aligned.

## ⚙️ 5. Advanced Business Logic & Performance
*   **Recursive Connection Management:** Implemented an auto-retry/detach mechanism in the Firebird service, ensuring that every connection used from the pool is returned (`db.detach()`) even in the event of a query error.
*   **Asynchronous Notification Triggers:** Optimized backend performance by moving email dispatch logic **outside of the Prisma Database Transactions**. This prevents "Transaction Timeouts" and ensures inventory data is saved in milliseconds.
*   **GS1-128 Parsing:** Built a custom parser using regex heuristics to extract Barcode, Lot Number, and Expiry Date from a single scanned medical string.
*   **RBAC & Security:** Integrated JWT-passport authentication to ensure every inventory change is cryptographically linked to a specific user.

## ⏱️ 6. Automated Risk Prediction & Compliance (Expiry Alerts)
Designed a proactive inventory monitoring system that triggers color-coded email alerts for medical products approaching their expiration dates.

*   **Dual-Threshold Architecture:**
    *   🔵 **180-Day Blue Alert:** Triggers exactly 6 months before expiry for inventory rotation planning.
    *   🟡 **30-Day Yellow Alert:** Triggers exactly 1 month before expiry indicating urgent consumption or disposal requirements.
*   **Automated Background Processing:** Configured a daily Cron job (`@nestjs/schedule`) executing at 08:00 AM server-time to scan the entire local MongoDB dataset against current date vectors.
*   **Dynamic Routing & Filtering:** Engineered logic to identify all active `LabSetup` configurations, map them to specific Warehouse IDs, and dynamically construct recipient lists directly from the Lab configurations, ensuring alerts reach the correct regional managers.
*   **Decoupled Service Layer:** Created the `StockExpiryNotificationService` to independently handle date math, stock isolation, and payload generation for the Mail module.

---

## 💡 The Result
By combining a **High-Concurrency Connection Pool** with a professional reporting layer and **Live Reactive Redux Tables**, I delivered a platform that maintains 100% data parity between legacy on-premise hardware and modern cloud infrastructure. The system actively predicts risks and automates the entire reporting chain from the warehouse to the executive office.
