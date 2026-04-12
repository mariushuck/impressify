# Impressify

Impressify is a micro-SaaS project for automated website compliance checks focused on GDPR and legal requirements (for example missing imprint/impressum pages, third-party integrations, and risky frontend patterns).

## Tech Stack

- Next.js (App Router) + React + TypeScript (strict mode)
- Tailwind CSS v4
- Supabase (`@supabase/ssr`) for auth and database integration
- Playwright for headless scanning automation

## Current Project Status

The repository is currently in a foundation stage:

- Next.js app scaffold is in place
- Supabase server/client helpers are implemented
- Middleware for session refresh is present
- Scanner module folder exists but scanning logic is not implemented yet

## Prerequisites

- Node.js 20+
- npm 10+
- A Supabase project (URL + anon key)

## Getting Started

1. Install dependencies:

```bash
npm install
```

2. Create an environment file:

```bash
cp .env.example .env.local
```

If `.env.example` does not exist yet, create `.env.local` manually with the variables listed below.

3. Start development server:

```bash
npm run dev
```

4. Open:

```text
http://localhost:3000
```

## Environment Variables

Add these values to `.env.local`:

```bash
NEXT_PUBLIC_SUPABASE_URL=your_supabase_project_url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_anon_key
```

## Scripts

- `npm run dev` - start local development server
- `npm run build` - create production build
- `npm run start` - run production server
- `npm run lint` - run ESLint

## Project Structure

```text
app/                # Next.js app router pages and layouts
components/         # Reusable UI components
lib/supabase/       # Supabase server/browser client helpers
lib/scanner/        # Playwright-based scanner logic (to be implemented)
public/             # Static assets
middelware.ts       # Request middleware (Supabase auth/session handling)
```

## Architecture Notes

- Use `@/*` absolute imports for internal modules.
- Prefer Server Components by default; use client components only when interactivity is required.
- Keep UI in `components`, routing in `app`, and business logic in `lib`.
- Use `@supabase/ssr` clients from `lib/supabase/client.ts` and `lib/supabase/server.ts`.

## Roadmap (Suggested)

1. Implement first scanner workflow in `lib/scanner` using Playwright.
2. Add scan persistence schema and writes to Supabase.
3. Create dashboard pages for scan history and issue details.
4. Add authentication flows and protected routes.
5. Add CI checks for lint/build.

## License

No license file is defined yet. Add a `LICENSE` file before public distribution.
