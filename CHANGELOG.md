# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- **Refundable Setup Fee Commercial Policy**:
  - Integrated ₹10,000 one-time setup fee across public landing pages, admin subscription portal, registration form, and 5-minute onboarding flow.
  - Formulated 100% refund guarantee upon 5 years (60 consecutive months) of continuous active platform usage.
  - Enforced strict 1-month (30 calendar days) post-5-year claim window with explicit non-liability / forfeiture terms in `public/terms.blade.php` and `LegalDocumentsSeeder`.

### Planned / In-Progress
- **Multi-Outlet Owner Panel**: Centralized executive analytics dashboard for multi-branch performance metrics, inventory movement tracking across outlets, cross-location staff scheduling, and consolidated revenue reports.
- **Staff QR Attendance & Payslip Engine**: Geofenced and QR-verified employee check-in/out, automated overtime calculation, late-punch audit approvals, monthly payroll ledger generation, and downloadable digital payslips.
- **Windows Desktop ESC/POS Wrapper**: Electron/Flutter desktop companion application providing offline-first local POS operation, background print spooling, and native USB/COM/LAN ESC/POS direct hardware socket printing.

## [0.9.1-beta] - 2026-11-22

### Added
- **Sales Employee Tracking & Referral Commission Engine**:
  - Implemented `SaasSalesAgent` model and Super-Admin control module (`/saas-admin/sales`) for employee sales tracking.
  - Generated employee sales IDs (e.g., `SALES101`) with tier-specific commission matrix: ₹50 for Solo & Starter, ₹300 for Growth & Pro.
  - Integrated optional `REF` referral code on registration form (`/register`) to cleanly attribute onboarded restaurants to employees.
  - Built comprehensive commission ledger tracking restaurants onboarded, plans sold, accrued commission, disbursements paid, and pending balances.
  - Implemented financial safety guard preventing agent deletion or deactivation while unsettled commission balance remains > ₹0.
- **14-Day All-Access Unlocked Trial Experience**:
  - Unlocked all advanced capabilities during trial (5 branches, 25 staff, 50 tables, 10 waiters, Kitchen Display System, recipe costing, and floor maps).
  - Added modular registration opt-outs for Storefront Website and Table QR Menu, cleanly gating unselected modules.
- **Transparent Commercial Architecture & Two-Column Checkout Overhaul**:
  - 1st-Year Annual Discount: 2 months free built into yearly billing with transparent renewal pricing.
  - 2.5% Payment Gateway Pass-through: Added at checkout with explicit payment processor disclosure and zero platform retention.
  - Zero Setup Fee Guarantee: One-time platform setup fee waived.
  - Responsive two-column checkout modal with pinned header, sticky action footer, itemized SAC 998313 (18% GST) calculation, and official B2B tax invoice inputs.
- **Continuous Showcase Synchronization**:
  - Configured GitHub Actions workflow to synchronize documentation and changelog to the public showcase repo on every push to `main` and on a daily recurring midnight schedule.

---

## [0.9.0-beta] - 2026-11-21

### Added
- **Multi-Tenant Database Isolation**:
  - Scoped data architecture with strict foreign-key tenant barriers (`tenant_id` & `branch_id`).
  - Automated session and query isolation via `TenantScope` and `TenantManager` service, preventing cross-tenant data leaks.
  - Safe subdomain and custom slug routing (`/{slug}`) with dedicated public landing pages and localized online menus.
- **SaaS Subscription Tier Engine**:
  - Multi-tier subscription management (`Free Starter`, `Solo`, `Pro`, and `Enterprise Custom`).
  - Automated feature gating, seat restrictions, and hardware entitlement verification via middleware (`EnsurePlanAllowsDevice`, `EnsureDesktopDevice`).
  - Integrated billing portal supporting recurring plan renewals, automated invoice generation, add-on feature packages, and structured cancellation/withdrawal workflows.
- **Super-Admin Control Panel**:
  - Comprehensive platform cockpit providing real-time system monitoring, tenant provisioning, and global telemetry.
  - Broadcast messaging engine with multi-channel dispatch (push notifications, email announcements, and in-app banners).
  - Economic settlements engine, automated subscription audit logging, tenant health telemetry, and self-healing diagnostics.
- **Parallel 3-Printer Hardware Bridge (`BKOT / KKOT / BILL`)**:
  - Simultaneous dual and triple thermal printer routing supporting Beverage KOT (`BKOT`), Kitchen KOT (`KKOT`), and Customer Checkout Bill (`BILL`).
  - Multi-interface protocol abstraction supporting Direct Network (RAW 9100 TCP sockets), WebUSB / Serial COM, and Bluetooth ESC/POS hardware.
  - Dedicated Virtual Printer test cockpit with instant job simulation, payload validation, and dispatch confirmation.
- **Sub-50ms Reactive POS Engine**:
  - Livewire 3 + Alpine.js interactive terminal with keyboard shortcuts, split-billing, running table holds, and dynamic UPI QR code generator.
  - Offline resilience with local sync queue (`offline-pos.js`) and IndexedDB fallback.
