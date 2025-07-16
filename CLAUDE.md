# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

BANDT is a full-stack Next.js starter template for building modern, type-safe web applications. It leverages Next.js 15 (App Router), tRPC, Drizzle ORM, better-auth, and shadcn/ui components.

## Key Technologies

- **Frontend**: Next.js 15, React 19, TypeScript, Tailwind CSS v4
- **Backend**: tRPC, Hono, PostgreSQL with Drizzle ORM
- **Authentication**: better-auth with Drizzle adapter
- **UI**: shadcn/ui components with dark mode support
- **State Management**: TanStack Query (React Query)
- **Validation**: Zod schemas
- **Package Manager**: pnpm

## Essential Commands

### Development
```bash
pnpm dev              # Start development server with Turbo
pnpm build            # Build for production
pnpm preview          # Build and preview production
pnpm start            # Start production server
```

### Code Quality
```bash
pnpm typecheck        # TypeScript type checking
pnpm check            # Run Biome linter
pnpm check:write      # Fix linting issues automatically
```

### Database
```bash
pnpm db:generate      # Generate Drizzle migrations
pnpm db:migrate       # Run migrations
pnpm db:push          # Push schema to database
pnpm db:studio        # Open Drizzle Studio
```

## Architecture

### Directory Structure
- `src/app/` - Next.js App Router pages and API routes
  - `(auth)/` - Authentication routes (login, signup)
  - `(protected)/` - Protected routes requiring authentication
  - `api/` - API routes (tRPC, auth handlers)
- `src/server/` - Server-side code (tRPC routers, database)
- `src/components/` - React components (ui/, auth/, layout/)
- `src/lib/` - Utilities and configurations
- `src/trpc/` - tRPC client setup

### Authentication Flow
- Uses better-auth with Drizzle adapter
- Middleware protects routes (`src/middleware.ts`)
- Session management integrated with tRPC context

### Data Flow
- Server components use `api` from `src/trpc/server.ts`
- Client components use tRPC hooks from `src/lib/trpc.ts`
- TanStack Query manages server state caching

## Development Patterns

### Component Types
- **Server Components**: Default, use for static content and server data fetching
- **Client Components**: Use `"use client"` directive, keep as small as possible
- **File naming**: `.client.tsx` for client components, `.tsx` for server components

### tRPC Usage
- Import `useTRPC` from `@/lib/trpc` for client components
- Use `useSuspenseQuery` with `trpc.query.queryOptions()`
- Use `useMutation` with `trpc.mutation.mutationOptions()`
- Server components use `api.query.method()` directly

### URL Parameters
- Use `nuqs/server` for type-safe search parameter parsing
- Create `search-params.ts` files with `createLoader` and parsers
- Pages receive `params` and `searchParams` promises

### File Naming Conventions
- **Components**: kebab-case (`button.tsx`, `space-list.tsx`)
- **Utilities**: kebab-case (`use-trpc.ts`, `auth-actions/`)
- **Pages**: `page.tsx`, `layout.tsx`, `loading.tsx`, `error.tsx`
- **API Routes**: `route.ts`
- **Route Groups**: `(auth)`, `(protected)` - don't affect URL
- **Dynamic Routes**: `[id]` - for dynamic segments
- **Private Folders**: `_components` - excluded from routing

### Form Handling
- Use `react-hook-form` with `zod` validation
- Leverage `@hookform/resolvers` for Zod integration

### Styling
- Use Tailwind CSS utility classes
- Leverage shadcn/ui components from `src/components/ui/`
- Support for dark mode via `next-themes`

## Database Schema

- All tables prefixed with `BANDT_`
- Auth tables: `user`, `session`, `account`, `verification`
- Schema files in `src/server/db/schema/`

## Important Notes

- No testing framework configured - add Vitest as needed
- Uses experimental Next.js features (node middleware enabled)
- Configured for Vercel deployment
- Environment variables validated via T3 env pattern
- Biome used for linting/formatting instead of ESLint/Prettier