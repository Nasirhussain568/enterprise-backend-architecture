# Enterprise Backend Architecture: Case Study in Legacy-Cloud Interoperability 🏗️

This repository documents the architecture and implementation of a high-scale enterprise platform designed to bridge the gap between legacy on-premise infrastructure and modern cloud-native systems.

## 🚀 Executive Summary
I architected and engineered a resilient backend solution addressing the critical challenges of **Legacy Data Modernization** (Firebird SQL to Cloud integration), **Automated Enterprise Reporting** (Excel/PDF generation), and **Real-Time Event Management**. 

The primary objective was to transform disconnected, raw data streams into actionable, high-fidelity business intelligence, ensuring **absolute data integrity** within a life-critical medical environment.

---

## 🛠️ Tech Stack Depth
![TypeScript](https://img.shields.io/badge/TypeScript-007ACC?style=for-the-badge&logo=typescript&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)
![NestJS](https://img.shields.io/badge/NestJS-E0234E?style=for-the-badge&logo=nestjs&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-4EA94B?style=for-the-badge&logo=mongodb&logoColor=white)
![Prisma](https://img.shields.io/badge/Prisma-2D3748?style=for-the-badge&logo=Prisma&logoColor=white)
![Firebird](https://img.shields.io/badge/Firebird-FF0000?style=for-the-badge&logo=Firebird&logoColor=white)
![Redux](https://img.shields.io/badge/Redux-764ABC?style=for-the-badge&logo=redux&logoColor=white)
![Ant Design](https://img.shields.io/badge/Ant%20Design-0170FE?style=for-the-badge&logo=antdesign&logoColor=white)

---

## 🏛️ 1. Hybrid Data Engineering (The "Sync Engine")
Designed and implemented a high-throughput synchronization layer to seamlessly orchestrate data flow between legacy systems and modern cloud backends.

*   **High-Concurrency Connection Pooling:** Eliminated race conditions and connection bottlenecks (`lazy_count` errors) by engineering a robust Firebird Connection Pool. This facilitates 10+ concurrent database transactions, allowing heavy background synchronization and high-frequency frontend requests to operate simultaneously.
*   **Bidirectional Deletion Reconciliation:** Implemented a 'Last-Seen' timestamp algorithm to resolve hard-deletion discrepancies in legacy systems. This accurately identifies and purges orphaned "ghost" records that no longer exist in the source Firebird instance.
*   **Dual-Database Cron Synchronization:** Established a highly available topology using automated **Cron Jobs** to periodically ingest and transform legacy data from on-premise **Firebird SQL** directly into **MongoDB**.
*   **On-Demand Manual Overrides:** Engineered secure, high-priority manual synchronization triggers using logic gates (`isManually`) to differentiate between scheduled maintenance and user-initiated updates.

## 📊 2. High-Fidelity Document Generation (Excel & PDF)
Medical compliance reporting demands uncompromising precision. I utilized **ExcelJS** to programmatically generate complex digital documents.

*   **Scientific Notation Mitigation:** Engineered strict protocols (`numFmt: '@'`) to prevent Excel from truncating 16-digit clinical barcodes into scientific notation (e.g., `1.08E+14`).
*   **Intelligent Expiry Monitoring:**
    *   🟡 **Gate 1 (Danger):** Automated Yellow (#ffff99) highlighting for items expiring within 30 days.
    *   🔵 **Gate 2 (Warning):** Automated Light Blue (#94dcf8) highlighting for items expiring within 6 months.
    *   🔴 **Variance Logic:** Visual Red (#ff5050) flagging for critical stock quantity discrepancies.

## ✉️ 3. High-Fidelity Communication Engine (Outlook-Proof)
Developed a notification service meticulously engineered to bypass the rendering constraints of legacy mail clients like Microsoft Outlook.

*   **Outlook "Gray-Scale" Mitigation:** Designed a table-based HTML architecture utilizing inline CSS and explicit `bgcolor` declarations to preserve **100% white backgrounds (#ffffff)** against Outlook’s aggressive shading.
*   **CID Logo Embedding:** Replaced Base64 strings with **CID (Content-ID) attachments** to ensure logos load instantly without being blocked.
*   **Brand Consistency:** Enforced strict visual hierarchy using **Pink Header Accents (#ED017F)** and bolded metadata labels.

## 🎨 4. Frontend Design Engineering & UI/UX Optimization
Resolved complex UI architecture challenges to deliver a fluid experience for clinical personnel.

*   **Omni-Stock Navigation Fix:** Eliminated critical navigation deadlocks by decoupling component loading states from warehouse-ID validation, allowing tables to render data during context switching.
*   **Column Standardization:** Implemented fluid CSS Grid architecture. Recalibrated widths (**Stock Code: 12%, Expiry: 10%, Status: 15%**) for pixel-perfect consistency.
*   **Ant-Design Realignment:** Corrected fixed-header misalignment in Ant-Design tables by enforcing `tableLayout="fixed"` and strict percentage constraints.
*   **Live Reactive Tables (Redux):** Engineered real-time state listeners using Redux for Variance Reports, enabling automatic UI hydration post-edit.

## 🔔 5. Real-Time Notification Ecosystem (Latest Work)
Engineered a unified, multi-channel notification framework for critical medical alerts.

*   **Contextual Deep-Linking:** Architected a sophisticated routing system (`notificationRoutes.js`) that enables **one-click navigation**. Clicking a notification automatically directs users to the specific entry (e.g., specific Variance ID), removing the need for manual searching.
*   **Schema & Logic Evolution:** Updated the **Prisma Schema** for persistent read/unread states. Enhanced the `NotificationsService` to dynamically construct context-aware messages based on warehouse ID and stock severity.
*   **UI Components:** Developed a high-performance `NotificationBell`, `NotificationList`, and `NotificationModal` system with reactive `NotificationItem` components.

## ⏱️ 6. Automated Risk Prediction (Expiry Alerts)
*   **Dual-Threshold Architecture:** Configured daily Cron jobs executing at 08:00 AM to scan global datasets against chronological vectors (180-Day / 30-Day thresholds).
*   **GS1-128 Parsing:** Developed a robust regex-driven heuristic parser to accurately extract Barcodes, Lot Numbers, and Expiration Dates from medical strings.

---

## 💡 The Result
By fusing a **High-Concurrency Connection Pooling** architecture with a **Real-Time Notification Ecosystem** and **Deep-Linked Frontend Routing**, I delivered an enterprise-grade platform that guarantees 100% data parity between legacy hardware and modern cloud infrastructure.

---

## 👥 Contributors
- **Lead Developer**: Saleem Ameen
- **Manager**: Ghayoor Haider
