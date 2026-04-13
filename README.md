# Impressify

Impressify is a micro-SaaS for automated website compliance checks. The product is designed to help identify GDPR and legal issues such as missing impressum/imprint pages, risky third-party integrations, and privacy-unfriendly frontend patterns.

## Stack

- Next.js 16 App Router with React 19 and TypeScript strict mode
- Tailwind CSS v4
- Supabase with `@supabase/ssr`
- Playwright for headless website scanning
- ESLint with `@stylistic/eslint-plugin`

## Status

The repository is in an early foundation stage. The app shell, Supabase helpers, and auth/session middleware are in place, while the actual scan orchestration and persistence flow are still to be implemented.

## Getting Started

### Prerequisites

- Node.js 20+
- npm 10+
- A Supabase project with a URL and anon key

### Install

```bash
npm install
```

### Configure environment

Create a local environment file and add your Supabase values. This repository does not include an `.env.example` file yet, so create `.env.local` manually.

```bash
NEXT_PUBLIC_SUPABASE_URL=your_supabase_project_url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_anon_key
```

### Run locally

```bash
npm run dev
```

Then open `http://localhost:3000`.

## Scripts

- `npm run dev` starts the development server
- `npm run build` creates a production build
- `npm run start` runs the production server
- `npm run lint` runs ESLint

## Project Structure

```text
app/             Next.js routes, layouts, and global styles
components/      Shared UI components
lib/scanner/     Scanner-related code and browser automation helpers
lib/supabase/    Supabase client helpers for server and client usage
public/          Static assets
middelware.ts    Request middleware for Supabase session refresh
```

## Architecture Notes

- Use `@/*` absolute imports for internal modules.
- Prefer Server Components by default and use client components only when interactivity is required.
- Keep routing in `app`, reusable UI in `components`, and business logic in `lib`.
- Use the Supabase clients from `lib/supabase/client.ts` and `lib/supabase/server.ts`.
- Keep scanner-specific code isolated under `lib/scanner`.

## Next Steps

1. Build the first Playwright-based scan workflow.
2. Add persistence for scan results and violations in Supabase.
3. Create dashboard pages for domains, scans, and findings.
4. Add authentication flows and route protection.
5. Add CI checks for linting and builds.
