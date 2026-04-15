# Cardiac Output / Advent One Backend System

This is the core NestJS backend for the Cardiac Output Inventory Management system. It handles real-time stock synchronization from Firebird databases to MongoDB, professional email automation, and warehouse management.

## 🛠 Features & Implementations

### 1. Global Warehouse Stock Sync
Developed a bulk synchronization system that:
- Identifies all active warehouses via SQL.
- Fetches real-time stock levels for each location.
- Processes thousands of stock records with automated logging.
- **New API Endpoint**: `POST /omni-stock/sync-all` to trigger a system-wide refresh.

### 2. Professional Email Notification Service
A robust implementation of `Nodemailer` optimized for corporate standards:
- **Multi-Server Support**: Configured for both Microsoft 365 (Office 365) and Gmail SMTP.
- **Security**: Fully integrated with Microsoft App Passwords and MFA protocols.
- **Deep Linking**: Implemented navigation links in emails to take users directly to specific entries in the frontend.

---

## 🐞 Bug Fixes & Optimization Log

### Microsoft Outlook Desktop Rendering Fix
**Issue**: The client reported "ghost" grey lines and background shading in the Outlook Desktop App.
**Solution**: 
- Migrated the template from modern CSS-in-DIV layout to a **Legacy Table-Based Layout**.
- Applied hard-coded `bgcolor="#ffffff"` attributes to every table cell to override Outlook's default Word-engine shading.
- Switched from Base64 images (which Outlook blocks) to **CID (Content-ID) Attachments** to ensure logos load instantly and safely.

### SMTP Authentication Debugging (Error 535 5.7.139)
**Issue**: New Microsoft accounts blocking automated script logins.
**Solution**: 
- Successfully identified and bypassed the "SmtpClientAuthentication is disabled" security wall.
- Implemented a temporary "Gmail Bridge" for design verification while waiting for client IT Admin SMTP unlocking.

---

## ⚙️ Setup & Environment Configuration

Create a `.env` file in the root directory:

```env
=
