# LIVE

**LIVE** is an emergency-response platform that connects people in danger with dispatchers and responders. A requester can raise an emergency in a few steps, share their location, and track the response in real time, while dispatchers, responders, administrators and auditors each get a workspace suited to their job.

> **Prototype status:** this version runs entirely in the browser. Authentication and data are simulated with browser-local mock data so that every role can be demonstrated without a backend. A production-oriented PostgreSQL/Supabase schema is included for the next stage.

---

## Features

- **Requesters**: create an emergency request in three steps, with automatic location capture, live map tracking, notifications and request history.
- **Dispatchers**: live operations queue, incident management, responder assignment and status updates.
- **Responders**: mission map and progress actions (acknowledge, en route, arrived, completed).
- **Administrators**: manage users, organisations, audit logs and prototype settings.
- **Auditors**: read-only incident and audit review.
- **Full status timeline**: every request status change is recorded, never overwritten.
- **Responsive UI**: role-specific navigation for desktop and mobile.

### Request lifecycle

```
submitted → received → assigned → en_route → arrived → closed
```

A request may also be `cancelled` (before assignment, or after an approved cancellation) or `rejected` (a reason is required and it stays visible to authorised staff).

---

## Tech stack

| Area | Technology |
|---|---|
| Framework | [Next.js](https://nextjs.org/) 16 (App Router) |
| UI | React 19, Tailwind CSS 3, Framer Motion, Lucide icons |
| Language | TypeScript (strict mode) |
| Utilities | `clsx`, `tailwind-merge`, `date-fns` |
| Auth helpers | `bcryptjs` |
| Backend (planned) | Supabase (PostgreSQL) via `@supabase/supabase-js` |

---

## Getting started

### Prerequisites

- Node.js 20 or newer
- npm

### Install and run

```bash
# 1. Clone the repository
git clone <your-repository-url>
cd live-response-app-redesign

# 2. Install dependencies
npm install

# 3. Start the development server
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) in your browser.

### Environment variables

The mock prototype works without a backend. To connect Supabase, create a `.env.local` file in the project root:

```env
NEXT_PUBLIC_SUPABASE_URL=your-project-url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-anon-key
SUPABASE_SERVICE_ROLE_KEY=your-service-role-key
```

> **Never commit `.env.local`** (it is already in `.gitignore`). `SUPABASE_SERVICE_ROLE_KEY` bypasses row-level security, so use it only in server-side code and never expose it to the browser.

### Scripts

| Command | Description |
|---|---|
| `npm run dev` | Start the development server |
| `npm run build` | Create a production build |
| `npm run start` | Run the production build |
| `npm run lint` | Lint the project with ESLint |
| `npm run typecheck` | Run the TypeScript compiler without emitting files |

---

## Demo accounts

Go to `/login`, select **Use a demo account**, choose an account (this fills in the email and password), then press **Sign in**.

| Workspace | Email | Password | Best for testing |
|---|---|---|---|
| Requester (active response) | `requester@live.co.za` | `LiveUser123!` | Live map, tracking, notifications, history |
| Requester (no active request) | `newrequester@live.co.za` | `LiveNewUser123!` | Automatic location and the three-step request flow |
| Dispatcher | `dispatcher@live.co.za` | `LiveDispatch123!` | Queue, incident management, assignment, status updates |
| Responder | `responder@live.co.za` | `LiveRespond123!` | Mission map and progress actions |
| Administrator | `admin@live.co.za` | `LiveAdmin123!` | Users, organisations, audit logs, settings |
| Auditor | `auditor@live.co.za` | `LiveAudit123!` | Read-only incident and audit review |

These are simulated in the browser and are **not** production credentials.

---

## Project structure

```text
app/
  page.tsx                  Public landing page
  login/ register/ forgot-password/
  app/
    requester/              Request creation, tracking, history and profile
    dispatcher/             Live operations, queue, resources and activity
    responder/              Mission, history and profile
    admin/                  Users, organisations, audit and settings
    auditor/                Read-only incident and audit review
components/
  auth/                     Session and demo-account components
  dashboard/                Shared operational metrics
  layout/                   Brand, public header, app shell and account sheet
  maps/                     Responsive mock live map
  requests/                 Shared request rows and status timeline
  ui/                       Buttons, fields, sheets, panels and skeletons
lib/
  mock-data.ts              Demo accounts and operational seed data
  mock-store.tsx            Functional browser-local workflow actions
  routes.ts                 Role-specific desktop and mobile navigation
  types.ts                  Shared TypeScript models
  utils.ts                  Formatting, routing and status helpers
supabase/
  schema.sql                Production-oriented database starting point
```

Route files stay small because navigation, sheets, the map, request rows, timelines and UI primitives are reusable components.

---

## Database

The recommended production structure is documented in [`DATABASE_ATTRIBUTES.md`](./DATABASE_ATTRIBUTES.md), with matching SQL in `supabase/schema.sql`. It covers 24 tables, including users, organisations, responder teams, vehicles, emergency requests, status history, assignments, notifications, sessions, and audit/security/integration logs.

Key design rules:

- UUID primary keys for records referenced outside the database.
- Relational columns for searchable data; JSONB only for flexible settings and equipment lists.
- Every status change is stored in `request_status_history`. A request and its first `submitted` row are created in one transaction.
- Passwords, JWTs and provider secrets are never stored or logged in plain form. Only hashes or secure secret references are kept.
- Timestamps are stored in UTC and converted for display.
- Production tables are accessed through protected Next.js server routes (planned custom JWT flow).

---

## Roadmap

- [ ] Replace the browser-local mock store with Supabase
- [ ] Server-side JWT sessions with revocation
- [ ] Real map, geocoding and routing providers
- [ ] Push, SMS and email notification delivery
- [ ] Hospital and responder system integrations

---

## Contributing

1. Create a feature branch from `main`.
2. Make your changes and run `npm run lint` and `npm run typecheck`.
3. Open a pull request describing what changed and why.

---

## Disclaimer

LIVE is a prototype for demonstration and development. It is **not** connected to real emergency services and must not be used to request emergency help. In a real emergency, contact your local emergency number.# LIVE
