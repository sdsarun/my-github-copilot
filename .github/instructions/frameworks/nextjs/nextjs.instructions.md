---
applyTo: '**/next.config.*,**/app/**,**/pages/**,**/_app.tsx,**/_app.ts,**/layout.tsx,**/layout.ts,**/page.tsx,**/page.ts'
---

# Next.js Conventions

## App Router vs Pages Router

- Use **App Router** (`app/`) for new projects — better caching, layouts, and server components
- Avoid mixing App Router and Pages Router in the same project
- Co-locate page-specific components alongside the route file — not in a global `components/`

## Server vs Client Components

- Default to **Server Components** — they run on the server and have no JS bundle cost
- Add `'use client'` only when you need: browser APIs, event handlers, `useState`, `useEffect`
- Keep Client Components as small and leaf-level as possible — push data fetching to Server Components

```tsx
// Server Component — no 'use client'
async function ProductPage({ params }: { params: { id: string } }) {
  const product = await fetchProduct(params.id); // Direct async/await — no useEffect
  return <ProductDetail product={product} />;
}
```

## Data Fetching

- Fetch in Server Components using `async/await` — no SWR or React Query needed for server data
- Use `cache()` from React to deduplicate concurrent fetches in the same render pass
- Use `unstable_cache` / `revalidatePath` / `revalidateTag` for ISR-style cache invalidation
- Never use `getServerSideProps` in App Router

## Route Handlers (API)

- `app/api/route.ts` for API routes — export named `GET`, `POST`, `PUT`, `DELETE` functions
- Validate request bodies with Zod before using the data
- Return `NextResponse.json()` with explicit status codes

## Performance

- Images: always use `<Image>` from `next/image` with `width` and `height`
- Fonts: use `next/font` — eliminates layout shift and self-hosts fonts
- Lazy-load heavy components: `const Chart = dynamic(() => import('./Chart'))`
- Keep route segments small — code-split at the page level by default

## Environment Variables

- Server-only vars: no prefix — never accessible to the browser
- Client-exposed vars: must start with `NEXT_PUBLIC_`
- Validate required vars at startup using Zod or `@t3-oss/env-nextjs`
