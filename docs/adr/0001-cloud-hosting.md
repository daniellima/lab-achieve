# ADR 0001: Choose Cloudflare Pages and Supabase for hosting

- **Status:** Accepted
- **Date:** 2026-10-14

## Context

Achieve is a personal todo-list web app being built with AI-assisted development. It needs a frontend and backend, should run on managed cloud services rather than self-hosted infrastructure, and should keep costs as low as practical. Self-hosting may be considered in the future.

The backend needs a database and may need authentication, file storage, and server-side endpoints. A general-purpose container host is not a requirement if managed backend functions can cover the app's needs.

## Options considered

### Cloudflare Pages + Supabase

Host the frontend as static assets on Cloudflare Pages. Use Supabase for managed Postgres, Auth, Storage, and Edge Functions.

- Cloudflare Pages has a free plan suitable for a small static frontend.
- Supabase has a free plan for development and small workloads; free projects may pause after one week of inactivity. Supabase Pro starts at $25/month and includes compute credits that cover a Micro database instance under current pricing.
- Supabase Edge Functions provide server-side TypeScript endpoints in a Deno-compatible runtime. They are functions, not arbitrary Docker containers.
- This separates frontend hosting from backend services and avoids needing to operate a server.

### Vercel + Supabase

Vercel provides a polished Git-based deployment workflow and is especially convenient for Next.js. Its free Hobby plan is intended for personal projects. Supabase provides the same managed backend services and costs described above. This is a strong alternative, particularly if the frontend uses Next.js.

### GitHub Pages + Supabase

GitHub Pages can host a static frontend at no charge and supports custom domains. It requires a build and publish workflow for many frontend frameworks. Published sites are public, and GitHub's usage policy says Pages should not be used for sensitive transactions such as sending passwords. This makes it a less appropriate fit for an app with a sign-in flow.

### Neon + Vercel

Neon provides managed Postgres with a free tier and usage-based paid plans. Combined with Vercel, it is a good choice when assembling a custom backend around Postgres. Compared with Supabase, it requires selecting or building additional services for authentication, file storage, and API behavior.

## Decision

Use **Cloudflare Pages for the frontend** and **Supabase for the backend**, including **Supabase Edge Functions** for server-side endpoints that are not suitable for direct browser-to-Supabase calls.

Use Supabase's managed Postgres, Auth, and Storage as needed. Start on free plans while developing and validate their inactivity, quota, and availability limits against actual usage. Upgrade only when those limits become a problem.

Edge Functions are appropriate for short-lived API requests, webhooks, and integrations. If the app later requires a persistent process, arbitrary container, unsupported runtime dependencies, or long-running/heavy jobs, deploy that workload to a separate managed container service and continue using Supabase as the data platform.

## Consequences

### Benefits

- Low initial hosting cost and no infrastructure to self-host or administer.
- Cloudflare Pages serves the frontend independently from the backend.
- Supabase bundles managed Postgres, authentication, storage, and server-side functions.
- Postgres and the Edge Functions' TypeScript/Web API patterns offer a path to migrate components later.
- Services can be moved independently: the frontend can change host without replacing the database, and a container backend can be introduced without replacing Supabase.

### Trade-offs and risks

- A free Supabase project may pause after a week of inactivity; an always-available paid project has a meaningful monthly base cost.
- Supabase Edge Functions run in Supabase's Deno-compatible runtime and have execution limits. They are not drop-in replacements for arbitrary Node.js servers or Docker containers.
- Using Supabase Auth, Storage, Realtime, gateway behavior, or SDK-specific features increases migration effort. Core business logic and database migrations should remain version-controlled, and provider-specific dependencies should be kept deliberate.
- Provider pricing and free-tier quotas can change. Review current pricing and set usage alerts before relying on a paid or usage-based tier.
- This architecture depends on two providers, so deployment, DNS, logs, and support are split between Cloudflare and Supabase.

## References

- [Cloudflare Pages limits and features](https://developers.cloudflare.com/pages/platform/limits/)
- [Supabase Edge Functions](https://supabase.com/docs/guides/functions)
- [Supabase Edge Function limits](https://supabase.com/docs/guides/functions/limits)
- [Supabase compute and disk](https://supabase.com/docs/guides/platform/compute-and-disk)
- [Supabase pricing](https://supabase.com/pricing)
- [GitHub Pages limits and usage policy](https://docs.github.com/en/pages/getting-started-with-github-pages/github-pages-limits)
- [Firebase pricing plans](https://firebase.google.com/docs/projects/billing/firebase-pricing-plans)
- [Neon pricing](https://neon.com/pricing)
- [Railway pricing](https://railway.com/pricing)
- [Vercel pricing](https://vercel.com/pricing)
