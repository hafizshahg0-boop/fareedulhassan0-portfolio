# Syed Fareed Ul Hassan — Portfolio Website

A professional, animated portfolio with a **hidden admin dashboard** (at `/admin`) that controls every word, photo, section and colour on the site.

- **Public site:** Home, About, Services, Portfolio, Videos, Gallery, Testimonials, Blog, Contact, Privacy.
- **Nothing is ever empty:** a section or page appears only once it has content. Add your first project and the Portfolio page and menu link appear automatically. The cards resize to fit the number of items.
- **Read-only for visitors:** nothing is editable on the public site. The dashboard isn't linked anywhere. You open it by typing `/admin` after your address.
- **Works before setup:** until the database is connected, the site shows the content from your CV (bundled in `/data`).

---

## Recommended free hosting (no monthly cost)

| What | Service | Cost | You get |
|---|---|---|---|
| Website | **Vercel (Hobby plan)** | Free | `https://your-name.vercel.app`, HTTPS, automatic deploys |
| Database, login, image storage | **Supabase (Free plan)** | Free | 500 MB database, 1 GB file storage |

**About a free domain:** Vercel gives you a free address such as `fareedulhassan.vercel.app`. A custom `.com` or `.pk` domain is never truly free; a `.com` costs roughly USD 10 a year. You can connect one later in Vercel → Settings → Domains without changing any code.

**The database won't fall asleep:** Supabase pauses free projects after about a week with no activity. This site includes a daily scheduled task (`vercel.json` → `/api/keep-alive`) that keeps it active automatically. Vercel's free plan allows daily scheduled tasks.

---

## Go live in about 20 minutes (no coding)

### Step 1 — Create the database (Supabase)
1. Go to **supabase.com** → *Start your project* → sign in with Google or GitHub.
2. Click **New project**. Name it `fareed-portfolio`, choose a strong database password and pick the closest region (e.g. *South Asia (Mumbai)*). Then click **Create**.
3. Open **SQL Editor** → **New query**. Paste the whole contents of `database/schema.sql` and click **Run**.
4. Open a **New query** again. Paste `database/seed.sql` and click **Run**. This loads your CV content and makes `fareedulhassan0@gmail.com` the admin.
5. Go to **Authentication → Users → Add user → Create new user**:
   - Email: `fareedulhassan0@gmail.com`
   - Password: choose one (at least 8 characters)
   - Tick **Auto Confirm User**
6. *(Recommended)* Go to **Authentication → Sign In / Providers** and turn off **Allow new users to sign up**. Only you will be able to log in.
7. Go to **Project Settings → API** (also shown as *Data API* / *API Keys*) and copy two values:
   - **Project URL** (looks like `https://abcd1234.supabase.co`)
   - **anon / public key** (a long text starting with `eyJ…`, or a key starting with `sb_publishable_`)

### Step 2 — Put the code on GitHub
1. Create a free account at **github.com** → **New repository** → name it `fareed-portfolio` → **Create repository**.
2. Upload the project folder. The easiest way is **GitHub Desktop**: *File → Add local repository* → choose this folder → *Publish*.
   - In the browser instead, use *uploading an existing file* and drag in the folders. GitHub's web uploader accepts 100 files at a time, so upload `app` and `components` first, then everything else.
   - Never upload `node_modules`, `.next` or a `.env.local` file.

### Step 3 — Publish on Vercel
1. Go to **vercel.com** → **Sign up with GitHub**.
2. **Add New… → Project** → choose `fareed-portfolio` → **Import**.
3. Open **Environment Variables** and add:

   | Name | Value |
   |---|---|
   | `NEXT_PUBLIC_SUPABASE_URL` | the Project URL from step 1.7 |
   | `NEXT_PUBLIC_SUPABASE_ANON_KEY` | the anon/public key from step 1.7 |
   | `NEXT_PUBLIC_SITE_URL` | `https://fareedulhassan.vercel.app` (your Vercel address) |
   | `CRON_SECRET` | any long random text, e.g. 40 random letters and numbers |

4. Click **Deploy**. After about two minutes, your site is live.
5. To choose the address, go to **Settings → Domains** and edit the `.vercel.app` name (e.g. `fareedulhassan`). Then update `NEXT_PUBLIC_SITE_URL` and click **Redeploy**.

### Step 4 — Manage your site
Open `https://your-address.vercel.app/admin`, sign in, and edit anything. Changes appear on the website **immediately**, with no redeploy needed.

---

## What you can manage in `/admin`

| Area | What it controls |
|---|---|
| Dashboard | Message count, item counts, quick actions |
| Inquiries | Contact-form messages (mark read or replied, reply by email, delete) |
| Profile & Contact | Name, roles (typing animation), introduction, **photo**, **CV file**, biography, details table, highlight numbers, email, phone, **WhatsApp** (adds a chat button), location, map, social links |
| Skills / Services / Experience / Education / Certifications | Add, edit, delete, hide or show, and reorder entries |
| Portfolio | Projects with cover image, gallery, rich description, tags, links, video, featured flag. Each project gets its own page |
| Videos | YouTube links (any format, including Shorts) or uploaded MP4/WebM files |
| Gallery | Photos with captions and categories, bulk upload, full-screen viewer |
| Testimonials | Real feedback with photo and star rating |
| Blog | Articles with cover, date, tags. Each one gets its own page |
| Media Library | Every uploaded file: upload, copy a link, delete |
| Home Sections | Show or hide each section and edit its headings |
| Pages & Menu | Menu labels, page titles and intros |
| Settings & SEO | Logo, **accent colours** (with presets), Google title, description and keywords, share image, footer, intro animation, buttons, privacy policy |
| Account & Admins | Change your password; add or remove other admin emails |

**Text formatting** (project and blog text): leave a blank line between paragraphs. Start a line with `## ` for a heading or `- ` for a bullet. Use `**bold**` and `[link text](https://…)`.

**Forgot your password?** Supabase → Authentication → Users → your user → *Send password recovery* or set a new password there.

---

## Run it on your own computer (optional)

Needs Node.js 20.9 or newer.

```bash
npm install
cp .env.example .env.local   # then paste your Supabase values into .env.local
npm run dev                  # opens http://localhost:3000
```

Other commands: `npm run build` (production build), `npm run typecheck`, `npm run lint`, `npm run seed:generate` (rebuild `database/seed.sql` from `/data`).

---

## Technology

- **Next.js 15 (App Router) + React 19 + TypeScript**
- **Motion for React** for animations, plus CSS animations. All of them respect the visitor's *reduce motion* setting.
- **Modular CSS design system** (`styles/`). Theme colours are editable from the dashboard.
- **Supabase**: PostgreSQL database, email/password login, file storage.
- **Security:** Row Level Security on every table, so only emails in the `admins` table can change data, enforced by the database itself. Every admin API route is checked on the server. All input is validated with Zod. Uploads are checked for type and size. Contact-form spam protection uses a hidden field and rate limiting. Inquiries are private to admins. The admin area is `noindex`.

### Project structure

```
app/                     pages, layouts, SEO (sitemap.ts, robots.ts)
  admin/                 hidden dashboard (login + every manager)
  api/                   contact form, public content, admin CRUD, uploads, keep-alive
components/              site sections and UI
  motion/                animation building blocks
  admin/                 dashboard components (generic ResourceManager, ContentEditor, uploaders)
config/                  admin-resources.ts (every editable collection), settings-forms.ts, theme, site config
styles/                  design tokens, typography, animations, admin styles
lib/                     database, auth, permissions, storage, validation, navigation helpers
types/                   shared TypeScript types
data/                    starting content from the CV (also the fallback if the database is down)
database/                schema.sql (tables + security) and seed.sql (starting content)
public/                  favicon, CV PDF, static media folders
design-reference/        the two reference screenshots and the design brief
```

### Notes on the requested file list
- **Lottie:** not used. The motion design is built with Motion for React and CSS, so no `public/animations/*.json` files are included.
- **API routes:** `app/api/admin/[resource]` provides validated create, read, update, delete and reorder for every collection. `app/api/admin/projects` is also present as an explicit route.
- **`package-lock.json`:** this is generated automatically by the first `npm install` (Vercel does this on deploy). It could not be generated in the environment where this project was written.
- **Extra admin pages** beyond the original list: Skills, Services, Experience, Education and Certifications, so every part of the CV is editable.
