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
![Ant Design](https://img.shields.io/badge/Ant%20Design-0170FE?style=for-the-badge&logo=antdesign&logoColor=white)

---

## 🏛️ 1. Hybrid Data Engineering (The "Sync Engine")
I engineered a high-performance synchronization layer to manage the data flow between legacy systems and modern backends.

*   **Dual-Database Cron Synchronization:** Architected a resilient Primary-Secondary database relationship. Implemented automated **Cron Jobs** to periodically ingest, transform, and sync legacy data from the on-premise **Firebird SQL (Secondary)** database directly into **MongoDB (Primary)**.
*   **On-Demand Manual Overrides:** Developed a high-priority manual trigger allowing users to initiate a sync outside the standard scheduler. Engineered logic-gate parameters (`isManually`) to distinguish between background maintenance and user-initiated updates.
*   **Sync State Persistent Monitoring:** Designed a **Singleton Metadata Model** (`LastSync`) using a fixed-ID document pattern in MongoDB. This provides the frontend with a lightweight reference to display the "Last Synced" heartbeat without querying heavy transaction tables.
*   **Idempotent Bulk Sync:** Designed "Check Previous, Update Next" logic. For bulk loads, the engine performs a heuristic search using a **composite key** (StockCode + LotNo + Warehouse + Expiry) to prevent duplicates.
*   **Manual Sequence Management:** Developed a custom `AutoIncrementService` to guarantee sequential, gapless IDs for Variance Reports and Stock Takes.
*   **Transactional Integrity:** Utilized `Prisma.$transaction` logic to ensure multi-warehouse loads are atomic, preventing corrupted data states.

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

*   **Outlook "Gray-Scale" Mitigation:** Engineered a custom table-based HTML architecture using inline CSS and explicit `bgcolor` attributes. This ensures that the branding remains high-fidelity and backgrounds stay **100% white (#ffffff)** even when viewed in Outlook’s aggressive Dark Mode.
*   **Dynamic Template Consolidation:** Refactored a high-complexity architecture (10+ files) into a single, high-performance TypeScript-based **Dynamic Template Engine**. This system uses a flexible "Detail-Array" pattern, allowing the backend to inject an unlimited number of custom fields (Labels/Values) without modifying the underlying HTML.
*   **Brand-Consistent Typography:** Implemented a specific visual hierarchy using **Pink Header Accents (#ED017F)** and bolded metadata labels to match the platform's clinical UI, ensuring a seamless user experience from the dashboard to the inbox.

## 🎨 4. Frontend Design Engineering & UI/UX Optimization
I solved critical UI challenges to ensure a seamless experience for clinical staff across different operating systems.

*   **Visual Hierarchy & Alignment:** Resolved structural layout shifts in the Permissions module. By recalibrating parent container padding and horizontal alignment, I ensured that the **'LABS SETUP'** interface elements align vertically with section headers, creating a balanced and professional white-space distribution.
*   **Layout Shift Mitigation (macOS):** Solved the "Always Show Scrollbars" layout shift issue on macOS by implementing `scrollbar-gutter: stable`. This prevents the table columns from jumping when the vertical scrollbar takes physical width.
*   **Ant-Design Table Realignment:** Fixed the "Separate-Table Misalignment" in fixed-header tables (`scroll={{ y: ... }}`). By recalibrating percentage-based widths to 100% and applying `tableLayout="fixed"`, I ensured the header and body columns remain perfectly aligned.

## ⚙️ 5. Advanced Business Logic & Performance
*   **Asynchronous Notification Triggers:** Optimized backend performance by moving email dispatch logic **outside of the Prisma Database Transactions**. This prevents "Transaction Timeouts" and ensures that inventory data is saved in milliseconds, while the email service processes in the background.
*   **GS1-128 Parsing:** Built a custom parser using regex heuristics to extract Barcode, Lot Number, and Expiry Date from a single scanned medical string.
*   **Dynamic Recipient Resolution:** Integrated the notification system with the `LabSetup` service to automatically resolve and deduplicate recipient lists based on specific laboratory configurations.
*   **RBAC & Security:** Integrated JWT-passport authentication to ensure every inventory change is cryptographically linked to a specific user.

---

## 💡 The Result
By combining the data sync engine with a professional, Outlook-compatible reporting layer, I delivered a platform that not only manages inventory but actively **predicts risks** and **automates the entire reporting chain** from the warehouse to the executive office.
