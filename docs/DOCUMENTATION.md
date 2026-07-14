# Acme Dashboard — Technical Documentation

Developer reference for the **nextjs-acme-dashboard** project.

## Overview

| Item | Detail |
|------|--------|
| **Repository** | [github.com/techwizh/nextjs-acme-dashboard](https://github.com/techwizh/nextjs-acme-dashboard) |
| **Live demo** | [nextjs-dashboard-ten-sigma-u5e22t1u16.vercel.app](https://nextjs-dashboard-ten-sigma-u5e22t1u16.vercel.app) |
| **Framework** | Next.js 16 (App Router) |
| **Language** | TypeScript |
| **Database** | PostgreSQL (Neon / Vercel Postgres) |
| **Auth** | NextAuth.js v5 (Credentials provider) |
| **Styling** | Tailwind CSS |
| **Hosting** | Vercel |

Built as the capstone project for the [Next.js Learn Dashboard course](https://nextjs.org/learn/dashboard-app) (Chapters 1–15).

---

## Architecture

```
nextjs-acme-dashboard/
├── app/                    # App Router pages and UI
│   ├── dashboard/          # Protected dashboard routes
│   ├── login/              # Login page
│   ├── lib/                # Data fetching, actions, types
│   └── ui/                 # Reusable components
├── auth.config.ts          # NextAuth route protection config
├── auth.ts                 # NextAuth providers and setup
├── proxy.ts                # Auth middleware (Next.js 16)
├── public/                 # Static assets (hero images, avatars)
└── docs/                   # User manual and this file
```

### Request flow

1. **Unauthenticated** users hitting `/dashboard/*` are redirected to `/login`.
2. **Login** uses a Server Action (`authenticate`) with email/password against the `users` table.
3. **Dashboard pages** fetch data from PostgreSQL via server components and Server Actions.
4. **Mutations** (create/update/delete invoices) use Server Actions with Zod validation.

---

## Tech stack

| Layer | Technology |
|-------|------------|
| Runtime | Node.js |
| Framework | Next.js 16.2.10 |
| UI | React 19, Tailwind CSS 3 |
| Database client | `postgres` (postgres.js) |
| Auth | next-auth 5.0.0-beta.25, bcrypt |
| Validation | Zod |
| Search debounce | use-debounce |
| Linting | ESLint 9, eslint-config-next |

---

## Environment variables

Copy `.env.example` to `.env` for local development.

| Variable | Required | Description |
|----------|----------|-------------|
| `POSTGRES_URL` | Yes | Pooled Postgres connection string |
| `POSTGRES_PRISMA_URL` | Yes | Prisma-compatible URL (from Neon/Vercel) |
| `POSTGRES_URL_NON_POOLING` | Yes | Direct connection for migrations/seeding |
| `POSTGRES_USER` | Yes | Database user |
| `POSTGRES_HOST` | Yes | Database host |
| `POSTGRES_PASSWORD` | Yes | Database password |
| `POSTGRES_DATABASE` | Yes | Database name |
| `AUTH_SECRET` | Yes | Session encryption secret (`openssl rand -base64 32`) |
| `AUTH_URL` | Yes | Auth callback base URL |

**Local example:**

```
AUTH_URL=http://localhost:3000/api/auth
```

**Production example:**

```
AUTH_URL=https://nextjs-dashboard-ten-sigma-u5e22t1u16.vercel.app/api/auth
```

Never commit `.env` to git.

---

## Local development

### Prerequisites

- Node.js 18+
- npm
- PostgreSQL database (Neon recommended)

### Install and run

```bash
npm install --legacy-peer-deps
npm run dev
```

Open [http://localhost:3000](http://localhost:3000).

If port 3000 is in use, Next.js picks the next available port (e.g. 3002).

### Other scripts

| Command | Description |
|---------|-------------|
| `npm run build` | Production build |
| `npm run start` | Run production build locally |
| `npm run lint` | Run ESLint |

---

## Database schema

Tables used by the app:

| Table | Purpose |
|-------|---------|
| `users` | Login credentials (email, bcrypt password) |
| `customers` | Customer profiles (name, email, image_url) |
| `invoices` | Invoices linked to customers (amount, status, date) |
| `revenue` | Monthly revenue for the dashboard chart |

Seed data includes demo user `user@nextmail.com` / `123456` and sample customers/invoices.

---

## Routes

| Route | Type | Description |
|-------|------|-------------|
| `/` | Static | Marketing / welcome page |
| `/login` | Static | Login form |
| `/dashboard` | Dynamic | Overview (cards, chart, latest invoices) |
| `/dashboard/invoices` | Dynamic | Invoice list with search and pagination |
| `/dashboard/invoices/create` | Static | Create invoice form |
| `/dashboard/invoices/[id]/edit` | Dynamic | Edit invoice |
| `/dashboard/customers` | Dynamic | Customer list with search |

Protected routes (`/dashboard/*`) require authentication via `proxy.ts`.

---

## Key files

| File | Role |
|------|------|
| `app/lib/data.ts` | SQL queries for dashboard and tables |
| `app/lib/actions.ts` | Server Actions: CRUD invoices, authenticate, signOut |
| `app/lib/definitions.ts` | TypeScript types |
| `auth.config.ts` | `authorized` callback — redirect logic |
| `auth.ts` | Credentials provider, user lookup |
| `proxy.ts` | Middleware matcher for route protection |
| `app/ui/search.tsx` | Debounced URL search param updates |

---

## Features implemented

- Server Components and streaming (Suspense + skeletons)
- URL-based search and pagination for invoices
- Server Actions for invoice CRUD with Zod validation
- Error handling (`error.tsx`, `not-found.tsx`, `useActionState`)
- Accessibility (ARIA labels, semantic HTML)
- NextAuth credentials login
- Metadata and Open Graph image
- ESLint (Chapter 13)

---

## Deployment (Vercel)

1. Push code to GitHub (`main` branch).
2. Import the repo in [Vercel](https://vercel.com).
3. Add all environment variables from `.env` to **Production**.
4. Set `AUTH_URL` to your production domain + `/api/auth`.
5. Deploy. Vercel uses `vercel.json`:

```json
{
  "installCommand": "npm install --legacy-peer-deps",
  "buildCommand": "npm run build"
}
```

`--legacy-peer-deps` is required because `next-auth@5.0.0-beta.25` peer-deps do not yet list Next.js 16.

After changing env vars, **Redeploy** for changes to take effect.

---

## Authentication

- **Provider:** Credentials (email + password)
- **Password storage:** bcrypt hashes in `users` table
- **Session:** NextAuth JWT
- **Protected paths:** All routes except `/`, `/login`, static assets, and API auth routes

Login flow is in `app/lib/actions.ts` → `authenticate()`.

---

## Troubleshooting

| Issue | Likely cause | Fix |
|-------|--------------|-----|
| Empty dashboard on Vercel | Missing `POSTGRES_URL` | Add env vars and redeploy |
| Login fails in production | Wrong `AUTH_URL` or empty DB | Set production `AUTH_URL`; seed database |
| Old UI after push | Vercel not redeployed | Check Deployments for latest commit |
| `npm install` peer conflict | next-auth vs Next 16 | Use `--legacy-peer-deps` |
| Port in use locally | Another dev server running | Stop other process or use assigned port |

---

## Related docs

- [USER_MANUAL.md](./USER_MANUAL.md) — End-user guide
- [Next.js Learn Course](https://nextjs.org/learn/dashboard-app)
- [NextAuth.js docs](https://authjs.dev)
- [Vercel Postgres / Neon](https://neon.tech)
