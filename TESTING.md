# Testing & verification report

This project was written in a sandbox that **could not download packages from npm**, so `npm install` and `next build` could not be run there. Everything below was tested; the first full install and build happen on Vercel at deploy time.

## What was tested

| Check | How | Result |
|---|---|---|
| TypeScript, all project files | `tsc --strict` against the real type definitions of React 19, Supabase JS + SSR, Motion and Zod (fetched from their source repositories), with small hand-written stand-ins for Next.js APIs | 0 errors |
| Database schema | `schema.sql` run twice on PostgreSQL 16 with Supabase-like `auth`/`storage` stand-ins | Pass, and safe to re-run |
| Seed data | `seed.sql` run twice | Pass; 13 skills, 4 services, 4 jobs, 3 qualifications, 2 courses, settings, admin email. No duplicates |
| Row Level Security | Queries as visitor, signed-in non-admin and admin | Visitors read only published rows and can send (but not read) inquiries; forged inquiry status is blocked; non-admins cannot change anything; admin email matching ignores letter case; hidden rows are invisible to visitors; storage uploads are admin-only |
| Validation & helpers | 29 unit tests (contact form, record schemas, settings, colour injection, uploads, paths, YouTube links, WhatsApp numbers, navigation rules) | 29 passed |
| Visual rendering | Real page components rendered in Chromium at 1440 px, 820 px and 390 px, both with only CV content and with sample content in every section | All pages render with no console errors and no horizontal scrolling. Empty sections and pages are hidden |
| Admin UI | Dashboard, list/grid managers, edit drawer, inquiries, settings and media rendered in Chromium with a mocked API | Renders with no errors |
| Reduced motion | Rendered with `prefers-reduced-motion: reduce` | Content shows without animation |

## Not tested here (needs the live services)
- `next build` itself, `next/image` optimisation and `next/font` downloads
- Real Supabase sign-in, file uploads to Storage and the API routes against a live project
- The Vercel daily keep-alive job

If the Vercel build shows an error, copy the red error text and it can be fixed quickly.
