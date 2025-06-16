## 🚀 Overview

This Full Stack Playbook is a living document that outlines the key technologies, development practices, and tooling strategies I use to build and maintain scalable, performant web applications across the stack. It serves both as a personal reference and a blueprint for collaborating engineers seeking insight into my workflow, architectural decisions, and development philosophy.

The playbook is grounded in the principle of balancing **developer productivity** with **system reliability**, placing a strong emphasis on rapid prototyping without compromising code quality, maintainability, or long-term scalability.

## 🧱 Core Technologies

A modern, streamlined stack focused on speed, developer experience, and operational efficiency:

### ⚙️ Framework

- **[NextJS](https://nextjs.org/)** (App Router) – for hybrid SSR/SSG, routing, and full-stack capabilities.
- **[Payload CMS](https://payloadcms.com/)** – acts as both a headless CMS and backend framework, tightly integrated into the NextJS runtime.

### 🎨 Frontend

- **[React](https://react.dev/)** – component-driven architecture with modern hooks-based logic.
- **[Tailwind CSS](https://tailwindcss.com/)** – utility-first styling for rapid UI iteration.
- **[shadcn/ui](https://ui.shadcn.com/)** – an extensible design system built on Tailwind.
- **[Framer Motion](https://motion.dev/)** – smooth, production-grade animations and transitions.

### 🧠 Backend

- **[Payload Local API](https://payloadcms.com/docs/local-api/overview)** – for high-performance server-side business logic and secure data operations.
- **[NextJS Server Actions](https://nextjs.org/docs/app/getting-started/updating-data)** – for handling server mutations with minimal overhead and built-in progressive enhancement.

### 🗃️ Database & ORM

- **[Payload Collections](https://payloadcms.com/docs/configuration/collections)** – abstracted ORM-like layer with schema and access logic defined at the CMS level.
- **[Supabase Postgres](https://supabase.com/database)** – scalable SQL database with real-time capabilities.
- **[Cloudflare D1](https://developers.cloudflare.com/d1/)** – lightweight, serverless SQL (pending support from Payload).

### 🗂️ Blob Storage

- **[Supabase Storage](https://supabase.com/storage)** – tightly integrated with Supabase auth and Postgres.
- **[Cloudflare R2](https://developers.cloudflare.com/r2/)** – S3-compatible, zero egress storage for public or CDN-backed assets.

### 🔐 Authentication & Access

- **[Payload Access Control](https://payloadcms.com/docs/access-control/overview)** - define granular restrictions based on the user, their roles (RBAC), document data, or any other criteria your application requires.
- **[Payload Auth](https://payloadcms.com/docs/authentication/overview)** – basic user auth, session-based login, and RBAC via access control functions.
- **[Supabase Auth](https://supabase.com/auth)** – for advanced features like OAuth, magic links, and multi-factor authentication.

### 📧 Email Infrastructure

- **[Resend](https://resend.com/)** – transactional email service with excellent DX and API ergonomics.
- **[React Email](https://react.email/)** – component-based email templating with shared styling logic across web and email UIs.

## 🧪 Code Quality & Automation

Maintaining code consistency and preventing regressions is a core priority. This section outlines the tools and practices I use to ensure a reliable development pipeline and a clean, readable codebase.

### 🛡️ Type Safety

- **[TypeScript](https://www.typescriptlang.org/)** – used across the entire codebase (frontend, backend, and config). Provides static typing, IDE support, and early error detection.
- Enforced with `strict: true` mode in `tsconfig.json` to maximise type coverage.
* **[zod](https://zod.dev/)** – for runtime schema validation and type-safe parsing of external input (e.g. form data, API requests, environment variables).
- **[t3-env](https://github.com/t3-oss/t3-env)** – validates environment variables at build and runtime using `zod` schemas. It explicitly separates **server-only**, **client-exposed**, and **shared** variables to prevent accidental leakage of secrets into the browser bundle.

### 🧹 Linting & Formatting

- **[BiomeJS](https://biomejs.dev/)** + **[Ultracite](https://www.ultracite.dev/)** - All in one code formatter and linter with a solid preset of rules for Typescript projects. Minimal config.
- **[Husky](https://typicode.github.io/husky/)** + **[lint-staged](https://github.com/lint-staged/lint-staged)** - Git hooks that run pre-commit checks only on staged files. Ensures fast feedback loops without slowing down the full repo.
- **[commitlint](https://commitlint.js.org/)** – validates commit messages against the [**Conventional Commits**](https://www.conventionalcommits.org/en/v1.0.0/) spec (e.g. `feat:`, `fix:`, `chore:`). Ensure a clean git history and better changelog generation.

### 🧬 Testing

- **[Vitest](https://vitest.dev/)** – blazing-fast test runner and assertion library. Supports native ES modules, watch mode, snapshot testing, mocking, and has built-in coverage tools.
- **[React Testing Library](https://testing-library.com/docs/react-testing-library/intro/)** – focuses on testing components from the user’s perspective, promoting accessible and behavior-driven test practices.

### 🔁 CI/CD and Deployment

- **[GitHub + GitHub Actions](https://github.com/features/actions)** – Used for version control and continuous integration. Provides a flexible workflow engine with ready-to-use templates for linting, type-checking, testing, and deploys.
  
- **[Cloudflare Workers](https://developers.cloudflare.com/workers/framework-guides/web-apps/nextjs/)** – Edge-first deployment platform with generous free tier, and solid NextJS support.

- **[Vercel](https://vercel.com/)** – Zero-config hosting with seamless integration for NextJS and instant preview deployments. First-class developer experience, but pricing can scale quickly.

## ✅ Good Practices

### 🗂️ Project Structure

NextJS relies heavily on file-based conventions for routing, layouts, and server actions. As a project scales, this can quickly lead to tightly coupled or disorganized code. To avoid that, I use a **modular, domain-based architecture** that encourages clear separation of concerns.

Each module represents a feature or domain and includes:
- UI components
- Hooks
- Helper functions
- Types and schemas
- API/server logic (if relevant)
- **Colocated tests** (e.g. `Component.test.tsx`, `hook.test.ts`) alongside the source code

This structure keeps everything contextually grouped, making the codebase:
- Easier to navigate and reason about
- More maintainable as it grows
- Friendly to testing, refactoring, and onboarding

Tests living close to their definitions help reduce friction during development, encourage better test coverage, and improve DX with quicker discoverability.

### 🗂️ Local Development

Before you start developing, make sure your environment is properly set up:

- **Node.js** – Use the latest LTS version, ideally managed via [nvm](https://github.com/nvm-sh/nvm) for consistency across projects.
- **pnpm** – Preferred package manager, enabled through [corepack](https://github.com/nodejs/corepack#readme).
- **IDE Setup** – Ensure your editor is configured with relevant plugins (TypeScript, BiomeJS, Tailwind IntelliSense, etc.) to match project conventions.
- **Local database** – Set up a working local database to avoid burning through production resources. Running a local database, isolates development changes, and protects your production or shared environments from accidental mutation. Guides for D1 and Supabase:
  - [Cloudflare D1](https://developers.cloudflare.com/d1/best-practices/local-development/)
  - [Supabase](https://supabase.com/docs/guides/local-development?queryGroups=package-manager&package-manager=pnpm)
  
## AI Tools

### 🎨 UI/UX Design
- **[v0.dev](https://v0.dev/)** – AI-powered UI generator trained on Next.js and shadcn/ui. Great for quickly prototyping components or generating layout ideas that integrate seamlessly with this stack.
- **[tweakcn](https://tweakcn.com/)** – A highly customizable theming tool for shadcn/ui. Offers finer control over visual styling and helps create a more unique, brand-aligned look without abandoning the design system.

### 🛢️ Supabase

If you're using Supabase, take advantage of their AI-enhanced developer tooling:

- **[MCP Server](https://supabase.com/blog/mcp-server)** – A language server that provides AI-native context awareness, helping you write Supabase queries and schema-related logic directly in your IDE.
- **[AI Prompts](https://supabase.com/docs/guides/getting-started/ai-prompts)** – Built-in prompt templates designed to improve productivity when using AI-assisted coding tools.

These tools significantly enhance the developer experience by offering intelligent completions, schema awareness, and smart code suggestions.