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
  - [Project Cancellation Batch Engine](#1-project-cancellation-batch-engine)
  - [Manager Content Alert System](#2-manager-content-alert-system)
  - [Intake Form Submission Receipts](#3-intake-form-submission-receipts)
- [Interfaces](#interfaces)
  - [Content Control Center](#1-content-control-center)
  - [Business Health KPI Dashboard](#2-business-health-kpi-dashboard)
  - [Lead Account Manager](#3-lead-account-manager)
- [Forms](#forms)
  - [Task Submission Intake Form](#1-task-submission-intake-form)
- [Field Types Legend](#field-types-legend)

---

## Overview

**Agency Operations OS** is a fully relational Airtable base designed for agencies to run their entire operation from a single source of truth. It connects clients to projects, projects to tasks, leads to sales activity, and team members to their workload — with built-in formulas, rollups, automations, interfaces, and forms to keep everything running automatically.

| Property | Details |
|---|---|
| **Base ID** | `app1jCUZQVT5LbXbp` |
| **Total Tables** | 8 |
| **Total Automations** | 3 |
| **Total Interfaces** | 3 |
| **Total Forms** | 1 |
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
| Status | Single Select | Pipeline stage e.g. New, Qualified, Closed-Lost |
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
| Content Status | Single Select | e.g. Draft, Scheduled, Ready to Publish |
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

### 1. Project Cancellation Batch Engine
> 📦 The Looping Engine

**Core Objective:** Handle systemic, multi-record data cascades simultaneously from a single operational trigger. Prevents manual status clean-up when a deal or account changes state.

#### Architecture

```
Trigger → Find Records → Repeating Group → Update Record (nested)
```

| Step | Type | Configuration |
|---|---|---|
| Step 1 | Trigger: When record matches conditions | Table: `Leads` · Condition: `Status` is `Closed-Lost` |
| Step 2 | Find Records | Table: `Tasks` · Where `Leads Link` contains `Record ID` from Step 1 |
| Step 3 | Repeating Group (For Each) | Input List: Step 2 → Records |
| Step 4 *(nested in Step 3)* | Update Record | Table: `Tasks` · Record ID: Step 3 → Current Item → Airtable Record ID · Field: `Task Status` → `Done` or `Cancelled` |

> ⚠️ **Critical Mapping Note:** The Record ID in Step 4 must be mapped from **Step 3 (Current Item)** — NOT from Step 2. Mapping from Step 2 directly only updates the first record. Mapping from Current Item inside the loop iterates through every found record successfully.

> ⚠️ **Trigger Note:** This automation fires on a **status change**, not an existing state. The record's status must transition TO the condition value to fire the trigger. To re-test, change the status away first, then change it back.

---

### 2. Manager Content Alert System
> 📢 The Quality Gate

**Core Objective:** Monitor asset pipelines or production delivery schedules, sending stylized dynamic stakeholder notifications exactly when an item clears validation rules.

#### Architecture

```
Trigger → Send Email
```

| Step | Type | Configuration |
|---|---|---|
| Step 1 | Trigger: When record matches conditions | Table: `Content Calendar` · Condition: `Content Status` is `🚀 Ready to Publish` |
| Step 2 | Send Email | To: Manager/Stakeholder email · Subject & Body with dynamic tokens from Step 1 |

#### Email Template

```
Subject: 🚨 PRODUCTION ALERT: Content Approved for Publishing!

Hi Manager,

An asset has been marked as fully cleared and is ready to go live.

Content Title:    [Token: Step 1 → Content Title]
Scheduled Date:   [Token: Step 1 → Publish Date]
Asset URL:        [Token: Step 1 → Asset Link]

Please review and execute distribution.
```

---

### 3. Intake Form Submission Receipts
> ⚡ The External Ingestion Gateway

**Core Objective:** Securely capture external data from public web form views and dynamically generate instant notification receipts to the submitting user upon successful entry.

#### Architecture

```
Trigger (Form Submitted) → Send Email
```

| Step | Type | Configuration |
|---|---|---|
| Step 1 | Trigger: When form is submitted | Table: `Tasks` · Form: `Task Submission Intake Form` |
| Step 2 | Send Email | To: Submitter's email token · Subject & Body with dynamic tokens from Step 1 |

#### Email Template

```
Subject: ✅ Task Successfully Logged: [Token: Step 1 → Task Name]

Hello,

This is an automated receipt confirming your task has been added to our operations pipeline.

Submission Details:
-------------------
Task Title:       [Token: Step 1 → Task Name]
Target Due Date:  [Token: Step 1 → Due Date]
Estimated Budget: $[Token: Step 1 → Task Cost]

Our operations team will review this asset shortly.
```

> ⚠️ **Important:** Submissions must be executed via the active **Preview mode URL** of the form. The internal Form Designer interface blocks live submissions and will not trigger the automation.

---

## Interfaces

### 1. Content Control Center
> 📅 The Scheduling & Approval Workspace

**Core Objective:** Provide a distraction-free, tableless layout for creators to schedule content and for managers to isolate and approve production assets — without digging through raw grid spreadsheet views.

#### Page Architecture

| Component | Configuration |
|---|---|
| Layout Type | Calendar Layout |
| Primary Table | `Content Calendar` |
| Calendar Field | `Publish Date` (blocks rendered per date) |
| Record Permissions | Add records: ON · Edit records: ON |
| Filter Sidebar | Targets `Content Status` field · Tapping a status (e.g. `🚀 Ready to Publish`) isolates matching records instantly |

> 💡 Team members can tap any calendar slot to log a new content piece directly from the interface without accessing the raw grid view.

---

### 2. Business Health KPI Dashboard
> 📊 The Executive Control Panel

**Core Objective:** Roll up data across different tables into high-level visual charts and numerical summaries. Enables stakeholders to gauge entire business health in under 5 seconds with zero exposed rows or text clutter.

#### Page Architecture

| Component | Type | Data Source | Computation | Filter | Label |
|---|---|---|---|---|---|
| KPI Element A | Summary / Number | `Clients` table | Sum of `Amount` (currency) | `Payment Status` is `✅ Paid` | `💰 TOTAL REVENUE GENERATED` |
| KPI Element B | Summary / Number | `Tasks` table | Count of records | `Task Status` is `Done` | `✅ COMPLETED WORK OPERATIONS` |
| Visual Metric | Bar Chart | `Tasks` table | Count per group | None | — |

**Bar Chart Configuration:**
- **X-Axis (Group By):** `Task Status`
- **Y-Axis:** Count — shows total operational backlog per delivery stage

> 💡 Uses a **Blank Canvas Template** to allow full custom positioning of all KPI elements.

---

### 3. Lead Account Manager
> 💼 The Detailed Record Focus Panel

**Core Objective:** Absolute user data isolation. Allows users to view and modify specific record fields safely without exposing adjacent rows or giving access to structural engineering formula properties.

#### Page Architecture

| Component | Configuration |
|---|---|
| Layout Type | Record Review Template (Split-Screen) |
| Primary Table | `Leads` |
| Left Panel | Navigation List View — displays lead profile names for quick selection |
| Right Panel | Isolated Detail Container |

**Right Panel Field Controls:**

| Field | Visibility | Editable |
|---|---|---|
| Lead Name | Visible | ✅ Yes |
| Status | Visible | ✅ Yes |
| Budget | Visible | ✅ Yes |
| Record IDs | Hidden | — |
| Autonumber counters | Hidden | — |
| Rollup formulas | Hidden | — |

> 💡 All hidden fields (Record IDs, raw counters, downstream rollup formulas) are dragged to the hidden panel to keep the interface clean and safe for non-technical users.

---

## Forms

### 1. Task Submission Intake Form
> 📋 The Data Ingestion Gateway

**Core Objective:** Provide a clean, foolproof web layout for external users to input new task requests. Standardizes incoming data and automatically handles information capture to prevent messy broken strings or manual copy-pasting.

#### Form Architecture

| Property | Configuration |
|---|---|
| Target Table | `Tasks` |
| Form View Name | `Task Submission Intake Form` |
| Access Mode | Public Preview URL |

#### Exposed Fields (Front-End)

| Field | Required | Notes |
|---|---|---|
| Task Name | ✅ Required | Forces user to provide an operational title before submitting |
| Due Date | ✅ Required | Uses Airtable's built-in calendar picker — guarantees valid date format |
| Task Cost | Optional | Restricted to numerical input only — prevents text symbols like `$450 dollars` from breaking downstream financial formulas |

#### Hidden Fields (Backend)

The following are dragged to the hidden sidebar and invisible to public users:

- Internal engineering parameters
- Calculated rollup fields
- Linked record columns
- System IDs and formula fields

> ⚠️ **Submission Note:** Always use the **Preview URL** to submit the form. The internal Form Designer view blocks live submissions and will not trigger the linked automation (Intake Form Submission Receipts).

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

*Documentation generated from live Airtable base — Agency Operations OS*  
*Generated: June 2026*
