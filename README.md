# 📱 Families Data Collection App — تطبيق جمع البيانات

A Flutter mobile application for **field researchers** to collect and submit family humanitarian data. Built to work fully offline and sync automatically when internet is available.

> 🖥️ **This app is one part of a two-component system.**
> Collected data is synced to Supabase and displayed in the **[Families Dashboard](link-to-dashboard-repo)** — a Next.js web admin panel used by supervisors and administrators.

---

## 🔗 System Overview

```
👷 Field Researcher
        │
        ▼
📱 Flutter App (This Repo)
   • Registers families offline
   • Syncs to Supabase when online
        │
        ▼
☁️  Supabase (PostgreSQL)
   • Single source of truth
   • Row Level Security per role
        │
        ▼
🖥️  Next.js Dashboard (Admin Panel)
   • Admins view, filter & export data
   • Works offline from local cache
```

---

## 👥 Who Uses What

| Role | Tool | Can Do |
|---|---|---|
| `field_researcher` | Flutter App | Register & edit families, sync data |
| `admin` | Dashboard | View, filter, export (read-only settings) |
| `super_admin` | Dashboard | Full access including app config |

---

## ✨ Features

- **Authentication** — Supabase email/password login
- **Family registration** — structured multi-step form covering:
  - Head of family (name, phone, age, job, marital status, chronic diseases, disabilities)
  - Wife (name, age, phone, pregnancy & breastfeeding status, chronic diseases, disabilities)
  - Children (name, age, gender, education level, national ID, chronic diseases, disabilities)
  - Housing (type, family status, original city & address, current address)
- **Edit & update** — modify existing family records
- **Offline-first** — all data saved locally first, synced when online
- **Sync status tracking** per record (`synced` / `pending` / `error`)
- **App config** — reads maintenance mode and force-update settings from Supabase on startup
- **RTL Arabic UI** — fully right-to-left interface

---

## 🛠 Tech Stack

| Layer | Technology |
|---|---|
| Framework | [Flutter](https://flutter.dev) 3.x |
| Language | Dart |
| Backend | [Supabase](https://supabase.com) |
| Local Storage | SQLite / Hive |
| Auth | Supabase Auth |

---

## 🔄 Sync Status

Every family record has a `sync_status` field:

| Status | Meaning |
|---|---|
| `synced` | Successfully uploaded to Supabase ✅ |
| `pending` | Saved locally, waiting for internet 🕐 |
| `error` | Sync attempt failed, needs retry ❌ |

---

## ⚙️ App Config

On every startup the app checks the `app_config` table in Supabase:

| Field | Effect |
|---|---|
| `is_maintenance = true` | Shows a maintenance screen, blocks access |
| `app version < min_app_version` | Forces the user to update |
| `update_url` | Redirects to the app store for the update |

---

## 🚀 Getting Started

### Prerequisites
- Flutter 3.x
- Dart SDK
- A [Supabase](https://supabase.com) project (shared with the dashboard)
- 
---

## 🗄 Database Schema

Shared with the dashboard. See the full schema in the
**[Dashboard README](link-to-dashboard-repo#-database-schema)**.

Key table: `families`

```
families
├── id              (uuid)
├── national_id     (text)
├── sector_name     (text)
├── head            (jsonb)
├── wife            (jsonb)
├── children        (jsonb[])
├── housing         (jsonb)
├── sync_status     (text)  ← managed by this app
└── created_at      (timestamptz)
```

---
<h3 align="center">واجهات نظام جمع البيانات</h3>

<p align="center">
  <img src="./screenshote/Main.png" width="350" alt="Main Pages" />
  <img src="./screenshote/Steps.png" width="350" alt="Steps to sign family" />
</p>

<p align="center">
  <i>نظام متكامل لتوثيق بيانات الأسر وتفاصيل النزوح بدقة عالية</i>
</p>
---
## 🔒 Supabase RLS

Field researchers can only:
- **Insert** new records
- **Update** records they created
- **Read** their own records

Admins and super admins have full read access via the dashboard.

---

## 🔗 Related

- **Admin Dashboard** — [link-to-dashboard-repo]
- **Supabase Project** — shared between both

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).
