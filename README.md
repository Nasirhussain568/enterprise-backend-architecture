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
![Prisma](https://img.shields.io/badge/Prisma-2D3748?style=for-the-badge&logo=Prisma&logoColor=white)
![Firebird](https://img.shields.io/badge/Firebird-FF0000?style=for-the-badge&logo=Firebird&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)

---

## 🏛️ 1. Hybrid Data Engineering (The "Sync Engine")
I engineered a high-performance synchronization layer to manage the data flow between legacy systems and modern backends.

*   **Idempotent Bulk Sync:** Designed "Check Previous, Update Next" logic. For bulk loads (`Warehouse ID = 0`), the engine performs a heuristic search using a **composite key** (StockCode + LotNo + Warehouse + Expiry) to prevent duplicates.
*   **Manual Sequence Management:** To ensure audit-ready compliance, I developed a custom `AutoIncrementService` to guarantee sequential, gapless IDs for Variance Reports and Stock Takes.
*   **Transactional Integrity:** Utilized `Prisma.$transaction` logic to ensure that multi-warehouse loads are atomic—preventing partial or corrupted data during network failures.
*   **Search Optimization:** Built a selective synchronization gate (`!scannedCode`) to keep real-time search latency **under 100ms**.

## 📊 2. High-Fidelity Document Generation (Excel & PDF)
Reporting in medical environments requires extreme precision. I leveraged **ExcelJS** to recreate complex physical paper trails digitally.

*   **Scientific Notation Fix:** Implemented string-forcing logic (`numFmt: '@'`) to prevent Excel from corrupting 16-digit barcodes into scientific notation (e.g., `1.08E+14`).
*   **Cell-Merging Logic:** Solved complex row-spanning challenges for clinical comments and Sales Rep headers across an 18-column (A-R) grid.
*   **Intelligent Expiry Monitoring:**
    *   🟡 **Gate 1 (Danger):** Automatic Yellow (#ffff99) for items expiring within 1 month.
    *   🔵 **Gate 2 (Warning):** Automatic Light Blue (#94dcf8) for items expiring within 6 months.
    *   🔴 **Variance Logic:** Visual Red (#ff5050) flagging for stock quantity mismatches.
*   **Auto-Adaptive Layouts:** Developed a dynamic row-height formula: `Height = (Characters / (ColumnWidth - Padding)) * LineFactor` to ensure 100% visibility of clinical notes.

## ✉️ 3. Communication & Template Engine (Advent One Logic)
Built a decoupled, responsive notification system using **MJML** and **Handlebars**.

*   **MJML Responsive Architecture:** Templates designed to maintain brand consistency across all mail clients (Outlook, Gmail, Apple Mail).
*   **Dynamic Data Binding:** Integrated Handlebars to inject live transaction data (Variance Reports) directly into HTML templates.
*   **User-Centric Audit Trails:** Built a comment aggregation logic that preserves item history: `[User Name] - [YYYY/MM/DD HH:mm] - [Content] |`.

## ⚙️ 4. Advanced Business Logic
*   **GS1-128 Parsing:** Built a custom parser using regex heuristics to extract Barcode, Lot Number, and Expiry Date from a single scanned medical string.
*   **Data Masking:** Implemented logic gates to treat "Open" status as null in exports to maintain uncluttered, professional documentation for management.
*   **RBAC & Security:** Integrated JWT-passport authentication to ensure every inventory change is cryptographically linked to a specific user for clinical accountability.

---

## 💡 The Result
By combining the **Advent One** notification logic with the **Cardiac Output** data sync engine, I delivered a backend that not only manages inventory but actively **predicts risks** (expiry tracking) and **automates the entire reporting chain** from the warehouse to the executive office.
