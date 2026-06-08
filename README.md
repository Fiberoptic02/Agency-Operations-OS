# 🏢 Agency Operations OS
> A comprehensive Airtable-based operating system for managing clients, projects, tasks, leads, team members, inventory, and content — all in one place.

---

## 📋 Table of Contents
- [Overview](#overview)
- [Base Structure](#base-structure)
- [Tables](#tables)
  - [Clients](#1-clients)
  - [Projects](#2-projects)
  - [Tasks](#3-tasks)
  - [Team Members](#4-team-members)
  - [Leads](#5-leads)
  - [Sales Activity](#6-sales-activity)
  - [Inventory](#7-inventory)
  - [Content Calendar](#8-content-calendar)
- [Table Relationships](#table-relationships)
- [Automations](#automations)
- [Field Types Legend](#field-types-legend)

---

## Overview

**Agency Operations OS** is a fully relational Airtable base designed for agencies to run their entire operation from a single source of truth. It connects clients to projects, projects to tasks, leads to sales activity, and team members to their workload — with built-in formulas, rollups, and automations to keep everything up to date automatically.

| Property | Details |
|---|---|
| **Base ID** | `app1jCUZQVT5LbXbp` |
| **Total Tables** | 8 |
| **Platform** | Airtable |
| **Last Updated** | June 2026 |

---

## Base Structure

```
Agency Operations OS
│
├── 👤 Clients
│     └── linked to → Projects
│
├── 📁 Projects
│     ├── linked to → Clients
│     ├── linked to → Tasks
│     └── linked to → Leads
│
├── ✅ Tasks
│     ├── linked to → Projects
│     └── linked to → Team Members
│
├── 👥 Team Members
│     └── linked to → Tasks
│
├── 🎯 Leads
│     ├── linked to → Projects
│     └── linked to → Sales Activity
│
├── 📞 Sales Activity
│     └── linked to → Leads
│
├── 📦 Inventory
│     └── (standalone — formula-driven stock tracking)
│
└── 📅 Content Calendar
      └── (standalone — content publishing tracker)
```

---

## Tables

### 1. Clients
> Stores all client records and links them to their associated projects.

**Table ID:** `tblVkvrIdZ5AN7F2F`

| Field Name | Type | Description |
|---|---|---|
| Client Name | Single Line Text | Primary field — name of the client |
| Email | Email | Primary contact email |
| Client Email | Email | Secondary/billing email |
| Status | Single Select | Current client status |
| Projects | Linked Record | Links to the Projects table |
| Created Date | Date | Date the client was added |
| Internal ID | Formula | Auto-generated unique identifier |

---

### 2. Projects
> Central hub of the OS. Each project ties together a client, tasks, budget, deadline, and health status.

**Table ID:** `tblypCw1Zf1H044Jy`

| Field Name | Type | Description |
|---|---|---|
| Project Name | Single Line Text | Primary field — name of the project |
| Project Type | Single Select | Category of the project |
| Status | Single Select | e.g. In Progress, Cancelled, Completed |
| Deadline | Date | Project due date |
| Total Project Cost | Rollup | Sum of all linked task costs |
| Project Health | Formula | Auto-calculated health indicator |
| Date Remaining | Formula | Days left until deadline |
| Project Countdown | Formula | Countdown display value |
| Client Link | Linked Record | Links to the Clients table |
| Tasks 2 | Linked Record | Links to the Tasks table |
| Leads | Linked Record | Links to the Leads table |
| Email (from Client Link) | Lookup | Client email pulled from Clients |
| Status (from Client Link) | Lookup | Client status pulled from Clients |
| Client Name (from Client Link) | Lookup | Client name pulled from Clients |

---

### 3. Tasks
> Individual work items assigned to team members and linked to projects.

**Table ID:** `tbl0vLrrU0j2J94VZ`

| Field Name | Type | Description |
|---|---|---|
| Task Name | Single Line Text | Primary field — name of the task |
| Task Status | Single Select | e.g. To Do, In Progress, Done, Cancelled |
| Due Date | Date | Task deadline |
| Task Cost | Currency | Cost associated with this task |
| Completed Date Stamp | Date | Date the task was marked complete |
| Priority | Formula | Auto-calculated priority level |
| Project Link | Linked Record | Links to the Projects table |
| Assigned Team | Linked Record | Links to the Team Members table |
| Project Name (from Project Link) | Lookup | Project name pulled from Projects |
| Member Name (from Assigned Team) | Lookup | Team member name pulled from Team Members |
| Role (from Assigned Team) | Lookup | Role pulled from Team Members |
| Client Email | Lookup | Client email pulled through Project link |

---

### 4. Team Members
> Tracks all agency staff and their assigned tasks.

**Table ID:** `tblai4PARzKkxUxug`

| Field Name | Type | Description |
|---|---|---|
| Member Name | Single Line Text | Primary field — full name |
| Role | Single Select | Team member's role/position |
| Tasks | Linked Record | Links to the Tasks table |

---

### 5. Leads
> Pipeline of potential clients and new business opportunities.

**Table ID:** `tblKtnXJAm1gnExmC`

| Field Name | Type | Description |
|---|---|---|
| Lead Name | Multi-line Text | Primary field — name of the lead |
| Email | Email | Lead contact email |
| Status | Single Select | Pipeline stage e.g. New, Qualified, Closed |
| Estimated Value | Currency | Potential deal value |
| Touchpoints | Count | Number of sales interactions logged |
| Sales Activity | Linked Record | Links to the Sales Activity table |
| Project Link | Linked Record | Links to the Projects table (when converted) |

---

### 6. Sales Activity
> Log of all touchpoints and interactions with leads.

**Table ID:** `tbljju2ndeSZhgiJ3`

| Field Name | Type | Description |
|---|---|---|
| Activity Name | Multi-line Text | Primary field — description of the activity |
| Date | Date | Date the activity occurred |
| Notes | Multi-line Text | Additional notes or outcomes |
| Lead Link | Linked Record | Links to the Leads table |

---

### 7. Inventory
> Tracks physical or digital inventory with automatic stock status alerts.

**Table ID:** `tblExHRKhQUJ6Du6I`

| Field Name | Type | Description |
|---|---|---|
| Item Name | Single Line Text | Primary field — name of the inventory item |
| Current Stock | Number | Current quantity in stock |
| Reorder Threshold | Number | Minimum stock level before reorder |
| Stock Status | Formula | Auto-displays stock health (e.g. OK / Low / Critical) |

---

### 8. Content Calendar
> Manages content production and publishing schedule.

**Table ID:** `tblv8xL1NRcotjiZE`

| Field Name | Type | Description |
|---|---|---|
| Content Title | Single Line Text | Primary field — title of the content piece |
| Publish Date | Date | Scheduled publish date |
| Content Status | Single Select | e.g. Draft, Scheduled, Published |
| Asset Link | URL | Link to the content asset or file |

---

## Table Relationships

```
Clients ──────────────── Projects
                         │       │
                       Tasks    Leads
                         │       │
                    Team Members  Sales Activity
```

| Relationship | Type |
|---|---|
| Clients → Projects | One-to-Many |
| Projects → Tasks | One-to-Many |
| Projects → Leads | One-to-Many |
| Tasks → Team Members | Many-to-Many |
| Leads → Sales Activity | One-to-Many |

---

## Automations

### 📦 Project Cancellation Batch Engine
> Automatically updates all linked tasks when a project is cancelled or changes status.

| Property | Details |
|---|---|
| **Trigger** | When a record matches conditions |
| **Trigger Table** | Projects |
| **Trigger Condition** | Status → is → In Progress (change to trigger) |
| **Step 1** | Find Records in Tasks where Project Link contains the triggered Project Record ID |
| **Step 2** | Repeat for each record in the found list |
| **Step 3 (inside loop)** | Update Record → set Task Status to `Cancelled` |

**How to trigger it:**
1. Change a Project's status away from "In Progress" (e.g. to "Planning")
2. Then change it back to "In Progress" (or to "Cancelled")
3. The automation fires and batch-updates all linked tasks

> **Note:** The trigger fires on a **status change**, not on an existing state. The project status must transition to trigger the automation.

---

## Field Types Legend

| Icon | Type | Description |
|---|---|---|
| 📝 | Single Line Text | Short plain text |
| 📄 | Multi-line Text | Long-form text |
| 📧 | Email | Email address field |
| 📅 | Date | Date picker |
| 🔢 | Number | Numeric value |
| 💵 | Currency | Monetary value |
| 🔗 | Linked Record | Relation to another table |
| 🔍 | Lookup | Value pulled from a linked table |
| 🔄 | Rollup | Aggregated value from linked records |
| ƒ | Formula | Auto-calculated field |
| 🔘 | Single Select | Dropdown with one choice |
| 🌐 | URL | Web link |
| 🔢 | Count | Counts linked records |

---

*Documentation auto-generated from live Airtable base — Agency Operations OS*
*Generated: June 2026*
