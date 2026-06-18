# Node.js / Frontend Project Analysis Rules

## Signature Detection
- `package.json` exists
- No `Assets/` directory (to distinguish from Unity which also has package.json at root)

## Scan Steps

### 1. Read package.json
```bash
read package.json
```
Extract:
- `name`, `version`, `description`
- `scripts` → build, dev, start, test commands
- `dependencies` → key frameworks:
  - `react`, `next`, `vue`, `svelte`, `angular` → frontend framework
  - `express`, `fastify`, `koa`, `hapi` → Node.js server
  - `tailwindcss`, `bootstrap`, `chakra-ui` → CSS framework
  - `prisma`, `typeorm`, `drizzle-orm` → ORM
  - `typescript` → TypeScript project
  - `vite`, `webpack`, `esbuild` → bundler
  - `jest`, `vitest`, `mocha` → test framework

### 2. Detect Project Type
| Clues | Type |
|---|---|
| `next` in deps | Next.js (full-stack or frontend) |
| `react` but no `next` | React SPA |
| `vue` or `nuxt` | Vue.js |
| `express` or `fastify` | Node.js API server |
| `vite` as devDep | Vite-based (likely React/Vue) |
| `astro` | Astro static site |
| `"type": "module"` | ESM project |

### 3. Map Directory Structure
Common patterns to look for:
| Directory | Common Role |
|---|---|
| `src/` | Main source code |
| `app/` | Next.js App Router pages |
| `pages/` | Next.js Pages Router or route files |
| `components/` | Reusable UI components |
| `lib/` or `utils/` | Utility functions |
| `api/` or `routes/` | API endpoints |
| `public/` | Static assets |
| `styles/` | CSS/SCSS files |
| `prisma/` | Database schema |
| `tests/` or `__tests__/` | Test files |

### 4. Find Entry Points
```bash
grep "start\|dev\|build" in package.json scripts section
```
Find the actual entry file:
- Next.js: `app/layout.tsx` or `pages/_app.tsx`
- Express: `grep "app.listen" in src/` or `grep "express()" in src/`
- Vite: `index.html` in root
- General: `glob **/index.{ts,js,tsx,jsx}` in src/

### 5. Check for Config Files
- `tsconfig.json` → TypeScript configuration
- `tailwind.config.*` → Tailwind CSS
- `.eslintrc.*` → Linting rules
- `.prettierrc` → Code formatting
- `.env.example` → Required environment variables
- `Dockerfile` → Container deployment
- `docker-compose.yml` → Multi-service setup
