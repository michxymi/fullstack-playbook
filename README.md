## 🚀 Overview

This Full Stack Playbook is a living document that outlines the key technologies, development practices, and tooling strategies I use to build and maintain scalable, performant web applications across the stack. It serves both as a personal reference and a blueprint for collaborating engineers seeking insight into my workflow, architectural decisions, and development philosophy.

The playbook is grounded in the principle of balancing **developer productivity** with **system reliability**, placing a strong emphasis on rapid prototyping without compromising code quality, maintainability, or long-term scalability. Every tool and service included has been carefully chosen to offer the best **value for money**.

## 🧱 Core Technologies

A modern, streamlined stack focused on speed, developer experience, and operational efficiency:

### ⚙️ Frameworks

- **[NextJS](https://nextjs.org/)** (App Router) – for hybrid SSR/SSG, routing, and full-stack capabilities.
- **[Payload CMS](https://payloadcms.com/)** – acts as both a headless CMS and backend framework, tightly integrated into the NextJS runtime.

### 🎨 Frontend

- **[React](https://react.dev/)** – component-driven architecture with modern hooks-based logic.
- **[Tailwind CSS](https://tailwindcss.com/)** – utility-first styling for rapid UI iteration.
- **[shadcn/ui](https://ui.shadcn.com/)** – an extensible design system built on Tailwind.
- **[Framer Motion](https://motion.dev/)** – smooth, production-grade animations and transitions.

### 🧠 Backend

- **[Payload Local API](https://payloadcms.com/docs/local-api/overview)** – enables ultra-fast data fetching by eliminating external HTTP requests—ideal for React Server Components and other server-side use cases.
- **[NextJS Server Actions](https://nextjs.org/docs/app/getting-started/updating-data)** – for handling server mutations with minimal overhead and built-in progressive enhancement.

### 🗃️ Database & ORM

- **[Payload Collections](https://payloadcms.com/docs/configuration/collections)** – abstracted ORM-like layer with schema and access logic defined at the CMS level.
- **[Cloudflare D1](https://developers.cloudflare.com/d1/)** – lightweight, serverless database with SQLite's SQL semantics. ([pending support from Payload](https://github.com/payloadcms/payload/pull/12537)).
- **[Turso](https://turso.tech)** - alternative serverless SQLite to consider, with built-in sync support.

### 🪣 Blob Storage

- **[Cloudflare R2](https://developers.cloudflare.com/r2/)** – S3-compatible, zero egress storage for public or CDN-backed assets.

### 🔐 Authentication & Access

- **[Payload Access Control](https://payloadcms.com/docs/access-control/overview)** - define granular restrictions based on the user, their roles (RBAC), document data, or any other criteria your application requires.
- **[Payload Auth](https://payloadcms.com/docs/authentication/overview)** – basic user auth, session-based login, and RBAC via access control functions.
- **[Clerk](https://clerk.com/)** – full-featured user management with OAuth, magic links, MFA, and prebuilt UI components.

### 📧 Email

- **[Resend](https://resend.com/)** – transactional email service with excellent DX and API ergonomics.
- **[React Email](https://react.email/)** – component-based email templating with shared styling logic across web and email UIs.

### 💳 Payments

- **[Stripe](https://stripe.com/)** - developer-friendly payments platform with robust APIs for handling transactions, subscriptions, and global commerce.

## 🧪 Code Quality & Automation

Maintaining code consistency and preventing regressions is a core priority. This section outlines the tools and practices I use to ensure a reliable development pipeline and a clean, readable codebase.

### 🛡️ Type Safety

- **[TypeScript](https://www.typescriptlang.org/)** – used across the entire codebase (frontend, backend, and config). Provides static typing, IDE support, and early error detection.
- Enforced with `strict: true` mode in `tsconfig.json` to maximise type coverage.
* **[zod](https://zod.dev/)** – for runtime schema validation and type-safe parsing of external input (e.g. form data, API requests, environment variables).
- **[t3-env](https://github.com/t3-oss/t3-env)** – validates environment variables at build and runtime using `zod` schemas. It explicitly separates **server-only**, **client-exposed**, and **shared** variables to prevent accidental leakage of secrets into the browser bundle.

### 🧹 Linting & Formatting

- **[BiomeJS](https://biomejs.dev/)** + **[Ultracite](https://www.ultracite.dev/)** - all-in-one formatter/linter for TypeScript with smart defaults and minimal setup.
- **[Husky](https://typicode.github.io/husky/)** + **[lint-staged](https://github.com/lint-staged/lint-staged)** - git hooks for running pre-commit checks on staged files only—fast feedback without scanning the whole repo.
- **[commitlint](https://commitlint.js.org/)** – validates commit messages using [**Conventional Commits**](https://www.conventionalcommits.org/en/v1.0.0/) (e.g. `feat:`, `fix:`, `chore:`) for clean git history and automated changelogs.

### 🧬 Testing

- **[Vitest](https://vitest.dev/)** – blazing-fast test runner and assertion library. Supports native ES modules, watch mode, snapshot testing, mocking, and has built-in coverage tools.
- **[React Testing Library](https://testing-library.com/docs/react-testing-library/intro/)** – focuses on testing components from the user’s perspective, promoting accessible and behavior-driven test practices.

### 🔁 CI/CD and Deployment

- **[GitHub + GitHub Actions](https://github.com/features/actions)** – version control with CI workflows for linting, testing, type-checking, and deploys.

- **[Cloudflare Workers](https://developers.cloudflare.com/workers/framework-guides/web-apps/nextjs/)** – edge-first deployment platform with generous free tier, and solid NextJS support.

## ✅ Good Practices

### 📁 Project Structure

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

### 💻 Local Development

Before you start developing, make sure your environment is properly set up:

- **Node.js** – use the latest LTS version, ideally managed via [nvm](https://github.com/nvm-sh/nvm) for consistency across projects.
- **pnpm** – preferred package manager, enabled through [corepack](https://github.com/nodejs/corepack#readme).
- **IDE Setup** – ensure your editor is configured with relevant plugins (TypeScript, BiomeJS, Tailwind IntelliSense, etc.) to match project conventions.
- **Local database** – set up a working local database to avoid burning through production resources:
  - [Cloudflare D1](https://developers.cloudflare.com/d1/best-practices/local-development/)
  - [Turso](https://docs.turso.tech/local-development)

Running a local database, isolates development changes, and protects your production or shared environments from accidental mutation.

## 🤖🛠️ AI Tools

A curated set of AI-powered tools that streamline UI design, theming, coding, and email template creation — perfect for accelerating development without compromising on quality or control.

- **[v0.dev](https://v0.dev/)** – AI-powered UI generator trained on NextJS and shadcn/ui. Great for quickly prototyping components or generating layout ideas that integrate seamlessly with this stack.
- **[tweakcn](https://tweakcn.com/)** – a highly customizable theming tool for shadcn/ui. Offers finer control over visual styling and helps create a more unique, brand-aligned look without abandoning the design system.
- **[email.new](https://new.email/)** - v0 but for email templates utilizing react email components.
- **[cursor](https://www.cursor.com/)** - AI-native IDE built on VS Code. Excellent for fast prototyping, refactoring, and in-editor AI assistance. Boosts productivity without leaving your coding flow.