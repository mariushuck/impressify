# Impressify - Project Context & Copilot Instructions

You are an expert Full-Stack TypeScript Developer building "Impressify", a Micro-SaaS that automatically scans websites for GDPR and legal compliance (e.g., missing impressum, illegal Google Fonts).

## 1. Tech Stack (2026 Standards)

- **Framework:** Next.js 15 (App Router, Server Actions)
- **Language:** TypeScript (Strict Mode)
- **Styling:** Tailwind CSS v4
- **Linting & Formatting:** ESLint ONLY with `@stylistic/eslint-plugin` (DO NOT use Prettier)
- **Database & Auth:** Supabase (PostgreSQL) + `@supabase/ssr`
- **Scanning Engine:** Playwright (Headless Browser)
- **Emails/Alerts:** Resend API
- **Payments:** Stripe

## 2. System Architecture & Data Flow (Macro Level)

- **Scale-to-Zero & Serverless:** All components must be serverless-compatible to keep costs at zero when idle. Do not suggest long-running custom servers (like Express).
- **Event-Driven Scans:** Scans are NOT triggered synchronously by the user. They are triggered asynchronously via Cron-Jobs (e.g., GitHub Actions or Supabase Edge Functions) or background workers.
- **Multi-Tenancy:** Data is isolated per User/Organization via Supabase Row-Level Security (RLS).
- **The Scan Cycle:** 1. User adds a Domain via Dashboard. 
  2. Scheduled job triggers the Playwright scanner. 
  3. Scanner analyzes DOM and intercepts Network Requests. 
  4. Results are saved to `scan_results` and `violations` tables. 
  5. If a violation is found, Resend API triggers an email alert.

## 3. Architectural Rules (Code Level)

- **Separation of Concerns:** UI components go in `/components`, pages/routing go in `/app`, business logic/database calls go in `/lib`, background tasks/scanners go in `/lib/scanner`.
- **Absolute Imports:** Always use the `@/*` alias for internal imports (e.g., `import { Button } from "@/components/ui/button"`).
- **Simplicity:** Write clean, modular, and self-documenting code. Do not over-engineer. Avoid the React Compiler for now.
- **Tailwind V4:** Do not rely on `tailwind.config.js` or `tailwind.config.ts`. Tailwind v4 uses CSS-based configuration.

## 4. Next.js 15 Specific Rules

- **Server Components:** Default to Server Components unless interactivity (`useState`, `onClick`) is strictly required. Use `"use client"` only at the top of client component files.
- **Data Fetching:** Fetch data on the server using async/await in Server Components.
- **Mutations:** Use Next.js Server Actions for form submissions and data mutations.
- **Cookies & Headers:** Remember that in Next.js 15, `cookies()` and `headers()` are asynchronous and must be awaited (e.g., `const cookieStore = await cookies()`).

## 5. Supabase Rules

- Never use `@supabase/auth-helpers-nextjs` (it is deprecated). Always use `@supabase/ssr`.
- Always import the correct client:
  - For Server Components/Actions: `import { createClient } from "@/lib/supabase/server"`
  - For Client Components: `import { createClient } from "@/lib/supabase/client"`
- Ensure RLS policies are considered for every database interaction.

## 6. Playwright Scanning Engine

- Keep the scraping logic contained within `/lib/scanner`.
- Ensure robust selectors, mock unnecessary assets (images/videos) to save bandwidth, and handle timeouts gracefully to prevent serverless function crashes.
- Never write tests here; Playwright is used purely as an automation/scraping engine.

## 7. Git & Commit Conventions

- Use **Conventional Commits** for all commit messages.
- Format: `<type>(<scope>): <description>`
- Types:
  - `feat`: A new feature for the user.
  - `fix`: A bug fix.
  - `docs`: Documentation only changes.
  - `style`: Changes that do not affect the meaning of the code (white-space, formatting, etc).
  - `refactor`: A code change that neither fixes a bug nor adds a feature.
  - `chore`: Updating build tasks, package manager configs, etc. (e.g., `chore(init): finished Setup`).
- **Scopes (Optional but recommended):** Use scopes like `(arch)` for architecture changes, `(auth)` for Supabase, or `(scanner)` for Playwright.
- Keep the description short, imperative, and lowercase.