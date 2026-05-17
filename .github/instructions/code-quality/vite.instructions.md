---
applyTo: '**/vite.config.*,**/vite.config.ts,**/vite.config.js,**/vite.config.mts'
---

# Vite Conventions

## Config Structure

```ts
// vite.config.ts
import { defineConfig, loadEnv } from 'vite';
import react from '@vitejs/plugin-react';

export default defineConfig(({ command, mode }) => {
  const env = loadEnv(mode, process.cwd()); // Only VITE_-prefixed vars are safe to load here

  return {
    plugins: [react()],
    resolve: {
      alias: { '@': new URL('./src', import.meta.url).pathname }
    },
    server: {
      port: 5173,
      proxy: {
        '/api': { target: env.VITE_API_URL, changeOrigin: true }
      }
    }
  };
});
```

## Environment Variables

- `VITE_`-prefixed vars are inlined into the browser bundle — safe for public config only
- Unprefixed vars are server-side and **never** accessible in browser code
- Never put secrets in `VITE_*` vars — they appear in the compiled output
- Use `import.meta.env.VITE_FOO` in client code, not `process.env`

## Dev Speed

- Vite uses native ESM in dev — cold start is fast because no bundling
- Hot Module Replacement (HMR) is precise — full page reload means the module is not HMR-compatible
- Dependency pre-bundling runs once and is cached under `node_modules/.vite` — clear with `vite --force` if stale

## Build Optimization

- Code-split with dynamic import: `const Chart = lazy(() => import('./Chart'))`
- Analyze bundle with `vite-bundle-analyzer` or `rollup-plugin-visualizer`
- Move large vendor deps to `build.rollupOptions.output.manualChunks` to control chunk boundaries

## Plugin Order

Plugin order matters:

1. Preprocessors and aliasing (resolvers)
2. Framework plugin (react, vue, svelte)
3. Transform plugins
4. Post-processing (compression, manifest)

Do not mix plugins from different build pipelines — Vite uses Rollup for production and its own server for dev.
