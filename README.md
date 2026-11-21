# Multi-Tenant Cloud POS & Restaurant Management SaaS — Currently In Active Build

[![Release](https://img.shields.io/badge/Release-v0.9.0--beta-emerald?style=for-the-badge&logo=git)](https://github.com/RG-Creators/cloud-pos-saas-showcase)
[![Architecture](https://img.shields.io/badge/Architecture-Multi--Tenant%20SaaS-blue?style=for-the-badge&logo=blueprint)](https://github.com/RG-Creators/cloud-pos-saas-showcase)
[![Tech Stack](https://img.shields.io/badge/Stack-Laravel%20|%20MySQL%20|%20Livewire%20|%20ESC%2FPOS-orange?style=for-the-badge&logo=laravel)](https://github.com/RG-Creators/cloud-pos-saas-showcase)
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
    |   Super-Admin   |             |  Tenant Portal  |             |  Customer Menu  |
    | Global Telemetry|             | & Billing Engine|             | & Online Orders |
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
- **Subscription Tier Lifecycle:** Gated tiers (`Free Starter`, `Solo`, `Pro`, `Enterprise Custom`) enforce device limits, table quotas, active order ceilings, and feature visibility.
- **Automated Billing & Recovery:** Integrated payment gateways manage automated cycle renewal, prorated upgrades/downgrades, and structured tenant cancellation/withdrawal workflows with full audit logging.

### 2. Parallel 3-Printer Hardware Bridge (`BKOT / KKOT / BILL`)
- **Station-Specific Dispatch:** Orders split dynamically across multiple thermal printers simultaneously:
  - **BKOT (Beverage KOT):** Routes bar and drink line items to station printers (58mm/80mm).
  - **KKOT (Kitchen KOT):** Dispatches food preparations to hot line/baking printers with item notes and modifier groups.
  - **BILL (Customer Receipt):** Dispatches final GST-compliant customer invoices with itemized totals, discount notes, and dynamic UPI payment QR codes.
- **Multi-Protocol Abstraction:** Native support for Direct RAW TCP/IP (port 9100), WebUSB/Serial COM, and Bluetooth ESC/POS hardware. Includes an integrated web-based Virtual Printer for testing without physical hardware.

### 3. Multi-Outlet Owner Analytics *(In Active Development)*
- **Consolidated Multi-Branch Telemetry:** Real-time visibility into gross sales, average table turn time, void/refund volumes, and peak order velocity across all brand outlets.
- **Centralized Menu & Inventory Management:** Unified recipe catalogs with central stock tracking and transfer logs between commissary and branch stores.

### 4. Staff QR Attendance + Ledger & Payslip Engine *(In Active Development)*
- **Contactless Punch Clock:** Staff verify shifts via dynamic QR code scanning and geofenced device checks.
- **Automated Late Auditing & Payroll Ledger:** Late-punch review workflow for managers, automated shift overtime computation, monthly salary ledgers, and exportable digital payslips.

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
| **Testing Coverage** | Automated PHPUnit Suite | Over 1,000+ test assertions covering calculation and security |

---

## Build Roadmap & Version History

| Version | Status | Milestone Highlights |
| :---: | :---: | :--- |
| `v0.9.0-beta` | **Current Release** | Multi-tenant DB isolation, SaaS subscription engine, Super-Admin cockpit, parallel 3-printer hardware bridge (BKOT/KKOT/BILL), virtual printer simulator. |
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
