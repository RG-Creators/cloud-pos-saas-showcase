# Multi-Tenant Cloud POS & Restaurant Management SaaS — Currently In Active Build

[![Release](https://img.shields.io/badge/Release-v0.9.0--beta-emerald?style=for-the-badge&logo=git)](https://github.com/RG-Creators/cloud-pos-saas-showcase)
[![Architecture](https://img.shields.io/badge/Architecture-Multi--Tenant%20SaaS-blue?style=for-the-badge&logo=blueprint)](https://github.com/RG-Creators/cloud-pos-saas-showcase)
[![Tech Stack](https://img.shields.io/badge/Stack-Laravel%20|%20MySQL%20|%20Livewire%20|%20ESC%2FPOS-orange?style=for-the-badge&logo=laravel)](https://github.com/RG-Creators/cloud-pos-saas-showcase)
[![Verification](https://img.shields.io/badge/Verified-1%2C024%20Assertions%20Passing-success?style=for-the-badge&logo=checkmarx)](https://github.com/RG-Creators/cloud-pos-saas-showcase)
[![License: Proprietary](https://img.shields.io/badge/License-Proprietary-red?style=for-the-badge&logo=shield)](https://github.com/RG-Creators/cloud-pos-saas-showcase)

---

> **Notice:** This repository serves as the public architectural showcase and technical roadmap for a high-throughput, enterprise-grade Multi-Tenant Restaurant POS & Management SaaS. The underlying production codebase is strictly proprietary and hosted in a private repository.

---

## Executive Summary

This platform is a cloud-native, multi-tenant Restaurant Operating System engineered for independent restaurants, cafes, bakeries, quick-service restaurant (QSR) chains, and cloud kitchens.

Engineered with sub-50ms reactive billing interactions, integer-safe currency computation, strict tenant isolation at the database layer, and low-latency physical ESC/POS hardware routing, the platform bridges cloud flexibility with mission-critical physical POS reliability.

---

## Core System Architecture & Modules

```
                                  [ CLOUD INFRASTRUCTURE ]
                                             |
             +-------------------------------+-------------------------------+
             |                               |                               |
             v                               v                               v
    +-----------------+             +-----------------+             +-----------------+
    |   Super-Admin   |             |  Tenant Portal  |             |  Modular Tenant |
    | Global Telemetry|             | & Billing Engine|             | Public Websites |
    +-----------------+             +-----------------+             +-----------------+
             |                               |                               |
             +-------------------------------+-------------------------------+
                                             |
                                [ Multi-Tenant ORM Scoping ]
                                [ (WHERE tenant_id = :id)  ]
                                             v
                             +-------------------------------+
                             |    MySQL 8.0 Primary Cluster  |
                             |  Strict Integer Paise Math    |
                             +-------------------------------+
                                             |
                         [ Real-Time SSE & ESC/POS Print Broker ]
                                             |
                                             v
                      +---------------------------------------------+
                      |         ON-PREMISE HARDWARE BRIDGE          |
                      |   (Direct Network TCP 9100 / WebUSB / BT)   |
                      +---------------------------------------------+
                               |              |              |
                               v              v              v
                          +----------+   +----------+   +----------+
                          |   BKOT   |   |   KKOT   |   |   BILL   |
                          | (Beverage|   | (Kitchen |   | (Checkout|
                          |  Bar 58) |   |  Line 80)|   |  Desk 80)|
                          +----------+   +----------+   +----------+
```

### 1. Multi-Tenant Database Isolation & Subscription Engine
- **Tenant Data Scoping:** Single-database multi-tenancy enforced at the Eloquent ORM layer via global tenant scopes (`tenant_id`, `branch_id`). Cross-tenant leakage is architecturally prohibited.
- **Subscription Tier Lifecycle:** Gated tiers (`Free Starter`, `Solo`, `Pro`, `Enterprise Custom`) enforce device limits, table quotas, active order ceilings, and feature visibility via kernel middleware.
- **Automated Billing & Recovery:** Integrated payment gateways manage automated cycle renewal, prorated upgrades/downgrades, and structured tenant cancellation/withdrawal workflows with full audit logging.

### 2. Sub-50ms Reactive POS Terminal & Money Math Invariants
- **Zero Floating-Point Arithmetic:** 100% of currency stored and calculated as integer `paise` (`BIGINT UNSIGNED`), eliminating rounding drift across taxes (CGST/SGST), discounts, and split bills.
- **Keyboard-First Interface:** Keyboard shortcuts (`F1-F12`, numeric keypads) enabling sub-50ms cashier order capture with Livewire 3 + Alpine.js.
- **Split-Billing & Running Tabs:** Itemized, cover-based, or fractional bill splits, plus persistent dining table tabs (`held_bills`).
- **Dynamic On-Bill UPI QR:** In-receipt dynamic UPI QR code generator enabling instant customer scan-and-pay with automatic POS settlement reconciliation.

### 3. Parallel 3-Printer Hardware Bridge (`BKOT / KKOT / BILL`)
- **Station-Specific Dispatch:** Orders split dynamically across multiple thermal printers simultaneously:
  - **BKOT (Beverage KOT):** Routes bar and drink line items to station printers (58mm/80mm).
  - **KKOT (Kitchen KOT):** Dispatches food preparations to hot line/baking printers with item notes and modifier groups.
  - **BILL (Customer Receipt):** Dispatches final GST-compliant customer invoices with itemized totals, discount notes, and dynamic UPI payment QR codes.
- **Multi-Protocol Abstraction:** Native support for Direct RAW TCP/IP (port 9100), WebUSB/Serial COM, and Bluetooth ESC/POS hardware. Includes an integrated web-based Virtual Printer (`/admin/virtual-printer`) for testing without physical hardware.

### 4. Kitchen Display System (KDS) & Floor Management
- **Live Ticket Pipeline:** Real-time order state progression (`Pending` $\rightarrow$ `In Kitchen` $\rightarrow$ `Ready` $\rightarrow$ `Served`) with station filters and audio chimes (`order_alert.wav`).
- **Visual Dining Layouts:** Multi-zone table floor plans with live seating states (`Vacant`, `Occupied`, `Billed`, `Reserved`) and contactless table QR code ordering.

### 5. Modular Public Website Engine (11 Modular Sections)
- **Zero-Code Storefront Builder:** Each restaurant manages an individualized public landing page (`/{slug}`) and digital menu (`/{slug}/menu`) with 11 toggleable components:
  1. Sticky Header Navbar
  2. Hero Welcome Banner
  3. About Us & Kitchen Story
  4. Live Food Catalog & Dietary Filters
  5. Ambience Studio Photo Gallery
  6. Chef's Signature Specials
  7. Customer Testimonials & Social Proof
  8. Store & Kitchen Operating Hours
  9. Interactive Location & Google Maps
  10. Announcement & Flash Sale Top Bar
  11. Footer, Social Links & Compliance Portals

### 6. Multi-Outlet Owner Analytics *(In Active Development)*
- **Consolidated Multi-Branch Telemetry:** Real-time visibility into gross sales, average table turn time, void/refund volumes, and peak order velocity across all brand outlets.
- **Centralized Menu & Inventory Management:** Unified recipe catalogs with central stock tracking and transfer logs between commissary and branch stores.

### 7. Staff QR Attendance + Ledger & Payslip Engine *(In Active Development)*
- **Contactless Punch Clock:** Staff verify shifts via dynamic QR code scanning and geofenced device checks.
- **Automated Late Auditing & Payroll Ledger:** Late-punch review workflow for managers (`confirmLatePunch`), automated shift overtime computation, monthly salary ledgers, and exportable digital payslips.

---

## Technical Specifications & Invariants

| Dimension | Specification | Implementation Detail |
| :--- | :--- | :--- |
| **Backend Framework** | Laravel 11 / PHP 8.3+ | Strict types, modular controllers, and service-layer design |
| **Reactive UI Layer** | Livewire 3 + Alpine.js | Sub-50ms DOM patching with zero heavy client framework bloat |
| **Styling & System** | Tailwind CSS | Strict neutral palette, dark/light compliance, DM Mono numbers |
| **Database Engine** | MySQL 8.0 InnoDB | Strict SQL mode, foreign-key indexing, zero soft-delete leaks |
| **Monetary Precision** | Integer Paise (`BIGINT`) | 100% elimination of floating-point arithmetic errors |
| **Hardware Protocol** | Native ESC/POS Standard | RAW byte serialization, cut commands, thermal rasterization |
| **Testing Coverage** | Automated PHPUnit Suite | Over 1,024 test assertions covering calculation, security, and gates |

---

## Build Roadmap & Version History

| Version | Status | Milestone Highlights |
| :---: | :---: | :--- |
| `v0.9.0-beta` | **Current Release** | Multi-tenant DB isolation, SaaS subscription engine, Super-Admin cockpit, parallel 3-printer hardware bridge (BKOT/KKOT/BILL), virtual printer simulator, modular website builder. |
| `v0.9.5-beta` | *In Progress* | Multi-outlet brand owner analytics dashboard, cross-branch menu syncing, and inventory transfer ledger. |
| `v1.0.0-rc1` | *Planned* | Staff QR attendance with geofence verification, automated shift ledger, and PDF payslip generator. |
| `v1.0.0-ga` | *Planned* | Native Windows/Electron desktop companion with background print spooler and offline-first cache sync. |

---

## Proprietary Software Notice

```
Copyright (c) 2026 RG. All Rights Reserved.

The source code, database schemas, proprietary algorithms, and architectural
implementations of this platform are strictly confidential and protected by
intellectual property and copyright laws. Unauthorized reproduction, decompilation,
or distribution of any part of this software is strictly prohibited.
```
