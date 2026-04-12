# Impressify - Project Context & Copilot Instructions

You are an expert Full-Stack TypeScript Developer building "Impressify", a Micro-SaaS that automatically scans websites for GDPR and legal compliance (e.g., missing impressum, illegal Google Fonts).

## 1. Tech Stack (2026 Standards)

- **Framework:** Next.js 15 (App Router)
- **Language:** TypeScript (Strict Mode)
- **Styling:** Tailwind CSS v4
- **Database & Auth:** Supabase (PostgreSQL) + `@supabase/ssr`
- **Scanning Engine:** Playwright (Headless Browser)

## 2. Architectural Rules (Micro-SaaS & Scale-to-Zero)

- **Separation of Concerns:** UI components go in `/components`, pages/routing go in `/app`, business logic/database calls go in `/lib`.
- **Absolute Imports:** Always use the `@/*` alias for internal imports (e.g., `import { Button } from "@/components/ui/button"`).
- **Simplicity:** Write clean, modular, and self-documenting code. Do not over-engineer. Avoid the React Compiler for now.
- **Tailwind V4:** Do not rely on `tailwind.config.js` or `tailwind.config.ts`. Tailwind v4 uses CSS-based configuration.

## 3. Next.js 15 specific Rules

- **Server Components:** Default to Server Components unless interactivity (`useState`, `onClick`) is strictly required. Use `"use client"` only at the top of client component files.
- **Data Fetching:** Fetch data on the server using async/await in Server Components.
- **Mutations:** Use Next.js Server Actions for form submissions and data mutations.
- **Cookies & Headers:** Remember that in Next.js 15, `cookies()` and `headers()` are asynchronous and must be awaited (e.g., `const cookieStore = await cookies()`).

## 4. Supabase Rules

- Never use `@supabase/auth-helpers-nextjs` (it is deprecated). Always use `@supabase/ssr`.
- Always import the correct client:
  - For Server Components/Actions: `import { createClient } from "@/lib/supabase/server"`
  - For Client Components: `import { createClient } from "@/lib/supabase/client"`

## 5. Playwright Scanning Engine

- Keep the scraping logic contained within `lib/scanner`.
- Ensure robust selectors and handle timeouts gracefully to prevent serverless function crashes.
- Never write tests here; Playwright is used purely as an automation/scraping engine.
