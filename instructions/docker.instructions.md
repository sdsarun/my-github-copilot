---
applyTo: '**/Dockerfile,**/Dockerfile.*,**/docker-compose*.yml,**/docker-compose*.yaml,.dockerignore'
---

# Docker Conventions

## Dockerfile

- Base images: use specific version tags, never `latest` — pin for reproducibility
- Multi-stage builds: separate build stage from runtime stage to reduce image size
- Non-root user: run the app as a non-root user in the final stage
- Copy only what is needed: copy dependency manifests first, install dependencies, then copy source
- Set `WORKDIR` before any `COPY` or `RUN`
- Use `.dockerignore` to exclude `node_modules`, `.git`, test files, and build artifacts

```dockerfile
# Good pattern
FROM node:22-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci --omit=dev
COPY . .
RUN npm run build

FROM node:22-alpine
WORKDIR /app
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/node_modules ./node_modules
USER node
EXPOSE 3000
CMD ["node", "dist/server.js"]
```

## docker-compose

- Use named volumes for persistent data — never anonymous volumes
- Define health checks on all service dependencies
- Use environment variable files (`.env`) — never hardcode secrets in compose files
- Set `restart: unless-stopped` on services that should auto-recover

## Security

- Scan images before deploying: `docker scout cves` or `trivy image`
- Do not mount the Docker socket inside containers unless absolutely required
- Never run production containers with `--privileged`
