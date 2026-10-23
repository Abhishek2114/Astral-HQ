<p align="center">
  <img src="apps/client/src/assets/hero.png" alt="AstralHQ dashboard preview" width="960" />
</p>

<div align="center">
  <img alt="React 19" src="https://img.shields.io/badge/React-19-61DAFB?style=for-the-badge&logo=react&logoColor=white" />
  <img alt="Node.js 20" src="https://img.shields.io/badge/Node.js-20-339933?style=for-the-badge&logo=node.js&logoColor=white" />
  <img alt="Express 5" src="https://img.shields.io/badge/Express-5-000000?style=for-the-badge&logo=express&logoColor=white" />
  <img alt="Prisma" src="https://img.shields.io/badge/Prisma-6-2D3748?style=for-the-badge&logo=prisma&logoColor=white" />
  <img alt="PostgreSQL" src="https://img.shields.io/badge/PostgreSQL-Database-4169E1?style=for-the-badge&logo=postgresql&logoColor=white" />
  <img alt="Vite" src="https://img.shields.io/badge/Vite-Build-646CFF?style=for-the-badge&logo=vite&logoColor=white" />
  <img alt="Tailwind CSS" src="https://img.shields.io/badge/Tailwind-CSS-06B6D4?style=for-the-badge&logo=tailwindcss&logoColor=white" />
  <img alt="License ISC" src="https://img.shields.io/badge/License-ISC-8A2BE2?style=for-the-badge" />
</div>

# AstralHQ

<p align="center">
  <strong>Mission Control for Modern Teams</strong>
</p>

<p align="center">
  AstralHQ is a full-stack workforce operations platform for managing projects, reviews, attendance, leave, task allocation, and team performance from a single workspace.
</p>

<p align="center">
  <a href="https://etharaai-ten.vercel.app">
    <img alt="Live Demo" src="https://img.shields.io/badge/Live-Demo-22c55e?style=for-the-badge&logo=vercel&logoColor=white" />
  </a>
  <a href="https://github.com/Abhishek2114/Ethara-AI-">
    <img alt="GitHub Repository" src="https://img.shields.io/badge/GitHub-Repository-181717?style=for-the-badge&logo=github&logoColor=white" />
  </a>
</p>

## Table of contents

- [Overview](#overview)
- [Problem it solves](#problem-it-solves)
- [Core features](#core-features)
- [Architecture](#architecture)
- [Technology stack](#technology-stack)
- [Repository structure](#repository-structure)
- [Quick start](#quick-start)
- [Environment variables](#environment-variables)
- [API surface](#api-surface)
- [Deployment](#deployment)
- [Security and implementation notes](#security-and-implementation-notes)
- [Future improvements](#future-improvements)
- [License](#license)

---

## Overview

AstralHQ is a monorepo project built with a React client and an Express + Prisma backend. It is structured like an internal operations dashboard for AI-driven or project-based teams, combining workforce management, review workflows, attendance, leave management, and analytics in one system.

The application is organized around a few practical operational concerns:

- project lifecycle and allocation
- task review and quality scoring
- attendance and leave tracking
- team performance summaries
- notifications, inbox, and activity feeds
- role-based access for reviewer, lead, and tasker users

The UI is built as a modern operational workspace with cards, metrics, dashboard widgets, and queue-based workflows rather than a generic CRUD interface.

## Problem it solves

Many teams still rely on fragmented tooling for staffing, review, attendance, and project visibility. This project consolidates those workflows into a single operational system so managers can:

- monitor taskers and project allocation
- review work quality and assign scores
- approve or reject leave requests
- track attendance status and operating patterns
- review performance trends in one dashboard

The implementation is designed as a practical internal team operations product rather than a static demo page.

---

## Core features

### 1) Team operations dashboard
- KPI cards for taskers, reviewed tasks, leave requests, and active projects
- manager-centric views for approvals and operational decisions
- project and team summaries in a single dashboard

### 2) Project management
- create and list projects
- project lifecycle and allocation status tracking
- team/project association through allocations
- work-item context for projects and review workflows

### 3) Task quality review
- review queue filtered by status
- quality rubric with explicit score values and comments
- reviewer attribution for completed work
- tracking of task completion and quality states

### 4) Attendance and leave lifecycle
- attendance records with statuses such as `PRESENT`, `ABSENT`, `IDLE`, and `ON_LEAVE`
- leave request creation and approval flows
- role-aware decisions between managers and taskers

### 5) Team analytics and visibility
- completion rate, quality score, and output summaries
- role-based dashboard data for project leads and reviewers
- polling-based refresh behavior for live updates

### 6) Notifications and activity feed
- activity event tracking for task, review, leave, and project actions
- read/unread notification structures
- operational timeline for the current workspace

### 7) Authentication and access control
- JWT access tokens and refresh-token rotation
- secure cookie-based refresh flow
- password hashing with `bcryptjs`
- protected routes and role-aware access handling

> Note: the AI insight flow in this repo is implemented as a data-driven heuristic layer that aggregates project metrics and creates insight records. It is not currently a production LLM integration in the code.

---

## Architecture

```mermaid
flowchart LR
    U[User] --> C[React + Vite Client]
    C -->|REST + Cookies + Socket.IO| A[Express API]
    A -->|Prisma ORM| P[(PostgreSQL or SQLite)]
    A -->|JWT + Refresh Tokens| AUTH[Auth Layer]
    A -->|Realtime events| S[Socket.IO]
    C -->|Protected routes| R[Role-based access]
```

### Backend structure
The backend is organized around feature modules and a versioned API:

- `/api/health`
- `/api/auth`
- `/api/dashboard`
- `/api/projects`
- `/api/leave`
- `/api/attendance`
- `/api/taskers`
- `/api/analytics`
- `/api/notifications`
- `/api/work-items`
- `/api/inbox`
- `/api/activity`
- `/api/ai`

### Frontend structure
The client uses route-based navigation and a protected app layout with pages for:

- auth/login flow
- dashboard
- taskers
- task review
- attendance
- leave
- projects
- analytics
- project detail views

---

## Technology stack

### Frontend
- React 19
- Vite
- Tailwind CSS v4
- React Router
- Axios
- Framer Motion
- Recharts
- `@dnd-kit` for drag-and-drop behaviors
- `cmdk` for command palette patterns
- `lucide-react` icons

### Backend
- Node.js 20+
- Express 5
- Prisma 6
- PostgreSQL
- SQLite support for local development
- Socket.IO
- Helmet
- `express-rate-limit`
- `express-validator`
- `jsonwebtoken`
- `bcryptjs`

### Data model
The Prisma schema includes operational domain entities such as:

- `User`
- `Project`
- `ProjectAllocation`
- `WorkItem`
- `LeaveRequest`
- `Attendance`
- `Notification`
- `Activity`
- `AiInsight`
- `Analytics`

This shows a real business-domain data model rather than a toy app schema.

---

## Repository structure

```text
Ethara-AI-/
├── .gitignore
├── DEPLOYMENT.md
├── README.md
├── package.json
├── package-lock.json
├── vercel.json
├── apps/
│   ├── client/
│   │   ├── .env.example
│   │   ├── package.json
│   │   ├── public/
│   │   ├── railway.toml
│   │   ├── src/
│   │   │   ├── assets/
│   │   │   ├── components/
│   │   │   ├── config/
│   │   │   ├── contexts/
│   │   │   ├── hooks/
│   │   │   ├── lib/
│   │   │   ├── pages/
│   │   │   ├── App.jsx
│   │   │   ├── main.jsx
│   │   │   ├── index.css
│   │   │   └── ...
│   │   └── vite.config.js
│   └── server/
│       ├── .env.example
│       ├── .env.sqlite
│       ├── Dockerfile
│       ├── package.json
│       ├── prisma/
│       │   ├── schema.prisma
│       │   ├── schema.sqlite.prisma
│       │   ├── migrations/
│       │   └── seed.js
│       ├── railway.toml
│       ├── scripts/
│       └── src/
│           ├── config/
│           ├── core/
│           ├── middleware/
│           ├── modules/
│           ├── routes/
│           ├── app.js
│           └── index.js
└── ...
```

---

## Quick start

### Prerequisites
- Node.js 20+
- npm
- Docker (recommended for local PostgreSQL)
- Optional: Vercel or Railway account for deployment

### 1) Install dependencies

```bash
git clone https://github.com/Abhishek2114/Ethara-AI-.git
cd Ethara-AI-
npm install
```

### 2) Configure environment files

```bash
cp apps/server/.env.example apps/server/.env
cp apps/client/.env.example apps/client/.env
```

For local development, the server includes a SQLite flow that is set up through the app scripts.

### 3) Start PostgreSQL locally (optional)

```bash
npm run docker:up
```

### 4) Run database migration and seed

```bash
npm run db:deploy
npm run db:seed
```

### 5) Start the application

#### Run both apps together
```bash
npm run dev
```

#### Run each app separately
```bash
npm run dev:client
npm run dev:server
```

Local URLs:
- Frontend: `http://localhost:5173`
- API: `http://localhost:3000/api`

### SQLite local setup
The server package includes a local SQLite workflow:

```bash
npm run setup:local
```

This creates the local SQLite database and generates Prisma artifacts for local development.

---

## Environment variables

### Root environment
The repository includes a root `.env.example` style setup for local Postgres configuration:

```env
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
POSTGRES_DB=astralhq
POSTGRES_PORT=5432

DATABASE_URL=postgresql://postgres:postgres@localhost:5432/astralhq?schema=public
```

### Server environment (`apps/server/.env`)

```env
NODE_ENV=development
PORT=3000
APP_NAME=Ethara AI API

DATABASE_URL="file:../data/dev.db"
# or:
# DATABASE_URL=postgresql://postgres:postgres@localhost:5432/astralhq?schema=public

JWT_ACCESS_SECRET=change-me-access-secret-min-32-chars-long
JWT_REFRESH_SECRET=change-me-refresh-secret-min-32-chars-long
JWT_ACCESS_EXPIRES_IN=15m
JWT_REFRESH_EXPIRES_IN=7d

BCRYPT_ROUNDS=12
CORS_ORIGIN=http://localhost:5173
COOKIE_SECURE=false

RATE_LIMIT_WINDOW_MS=900000
RATE_LIMIT_MAX=200
AUTH_RATE_LIMIT_MAX=20
TOKEN_CLEANUP_INTERVAL_MS=3600000
```

### Client environment (`apps/client/.env`)

```env
VITE_API_URL=http://localhost:3000
```

---

## API surface

The app exposes a versioned API via `/api` and organizes routes by feature area.

| Module | Purpose |
|---|---|
| `/auth` | registration, login, refresh, logout, and profile access |
| `/dashboard` | summary metrics and manager dashboard state |
| `/projects` | project listing, creation, and project context |
| `/leave` | leave request lifecycle and status updates |
| `/attendance` | attendance tracking and check-in workflows |
| `/taskers` | team staffing and tasker performance summary |
| `/analytics` | team analytics and score trends |
| `/notifications` | notifications and unread state |
| `/work-items` | task status and review lifecycle |
| `/inbox` | inbox operation and action data |
| `/activity` | operational activity log |
| `/ai` | insight generation endpoints |

### Representative endpoints

```http
POST /api/auth/register
POST /api/auth/login
POST /api/auth/refresh
POST /api/auth/logout
GET  /api/auth/me

GET  /api/dashboard/stats
GET  /api/projects
POST /api/projects
GET  /api/projects/:id
PATCH /api/leave/:id/status
POST /api/attendance/check-in
GET  /api/analytics/team
GET  /api/work-items
PATCH /api/work-items/:id/review
GET  /api/health
GET  /api/ai/insights
POST /api/ai/insights/generate
```

The client uses Axios with automatic refresh-token retry logic and sends the access token via the Authorization header when available.

---

## Demo credentials

The seeded demo users are intentionally included in the seed script and are safe to use in local development.

| Role | Email | Password |
|---|---|---|
| Quality Reviewer | `abhishek.singh23@ethara.ai` | `Admin123!` |
| Project Lead | `piyush.tomar@ethara.ai` | `Admin123!` |
| Tasker | `tasker1@ethara.ai` | `Member123!` |

---

## Deployment

This project is configured for a modern monorepo deployment model and includes Vercel and Railway guidance.

### Local Docker deployment
The root `docker-compose.yml` is used for PostgreSQL setup:

```bash
npm run docker:up
```

### Vercel + Railway deployment
The repository includes:

- `vercel.json`
- `DEPLOYMENT.md`
- `apps/server/railway.toml`
- `apps/client/railway.toml`

The repo’s deployment guide outlines the pattern:

1. Create a Railway PostgreSQL service.
2. Copy the `DATABASE_URL` and set it in the app environment.
3. Import the repository into Vercel as a single deployment.
4. Set `JWT_ACCESS_SECRET`, `JWT_REFRESH_SECRET`, `CORS_ORIGIN`, and `COOKIE_SECURE` in production.
5. Run Prisma migrations against the production database.
6. Deploy the app and use the app URL as the frontend origin.

The project is designed to support both local development and hosted deployment, though some realtime/socket behavior depends on the hosting environment.

---

## Security and implementation notes

The current codebase includes several real engineering safeguards:

- JWT access + refresh token flow
- refresh token hashing and rotation
- secure cookie handling for authentication
- password hashing with `bcryptjs`
- request validation via `express-validator`
- rate limiting for auth endpoints and general requests
- Helmet middleware for HTTP hardening
- CORS configuration for controlled origins
- Prisma-based database access with model constraints
- protected client routes and role-aware server logic

This should be evaluated as a development-grade internal operations platform, not a fully hardened production system without further review and deployment hardening.

---

## Future improvements

The following are realistic roadmap items aligned with the current architecture:

- add more granular project comments and approvals
- improve task assignment and workload balancing
- expand historical analytics and date-range reporting
- add richer real-time collaboration and notifications
- strengthen AI insight generation with external model integrations
- add test coverage for API and frontend flows
- improve audit logs, soft deletes, and permission review controls

---

## License

This project is licensed under the ISC license.

---

<p align="center">
  <strong>Built for operational clarity.</strong>
</p>

<p align="center">
  Explore the project, run it locally, and review the workflow behind the dashboard.
</p>
