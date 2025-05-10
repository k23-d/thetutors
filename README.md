// Monorepo Project Setup for Full-Stack SaaS Marketplace

// File: README.md

## SaaS Marketplace Monorepo Overview

### Tech Stack
- **Frontend**: Nuxt 3 (Vue 3, TypeScript, TailwindCSS)
- **CMS**: Sanity.io
- **Auth & DB**: Supabase (RLS-enabled)
- **Hosting**:
  - Vercel (static frontend + CMS)
  - Railway (backend APIs + n8n workflows)
- **CI/CD**: GitHub Actions

### Directory Structure
```bash
/apps
  /website        # Nuxt 3 app (public site + user dashboard)
  /backend        # FastAPI server for API logic & n8n routing
  /cms            # Sanity.io Studio project
/packages
  /shared         # Shared TypeScript types, utility functions, constants
/n8n              # n8n project files, JSON workflows, config
```

### Key Features

#### 🖥️ Public Marketing Site (CMS-editable via Sanity)
- Homepage
- About / What We Do
- Products Page (AI tools)
- How It Works
- Contact Us

#### 👤 User Auth Flow (Supabase)
- Email/password + Google OAuth + magic link
- Persistent session with JWT
- Role-based access: Admin, Free, Paid

#### 📊 Post-login Dashboard
- Displays available tools by role
- Triggers n8n workflows via backend API
- Optional: usage & credit tracking

#### 🛠️ Admin Panel
- View/manage users and roles
- Manage tool metadata (editable via Sanity)
- Add Markdown/WYSIWYG docs (via Sanity)

#### 🔌 Integrations
- Stripe/LemonSqueezy integration (stubbed)
- Supabase storage (for file-based logs or inputs)
- n8n triggered via webhook endpoints in backend

### GitHub Actions Setup
- `/apps/website`: deploy to Vercel
- `/apps/backend`: deploy to Railway
- `/n8n`: optional deployment script

---

## Development Setup (VS Code Friendly)

### 1. Clone the Monorepo & Install Dependencies
```bash
git clone https://your-repo-url.git
cd your-repo
pnpm install
```

### 2. Open in Visual Studio Code
```bash
code .
```

### 3. Setup Environment Files
Create the following `.env` files:
- `/apps/website/.env`
- `/apps/backend/.env`
- `/apps/cms/.env`

Populate them with:
```env
SUPABASE_URL=...
SUPABASE_ANON_KEY=...
SUPABASE_SERVICE_ROLE=...
OPENAI_API_KEY=...
SANITY_PROJECT_ID=...
SANITY_DATASET=...
SANITY_TOKEN=...
n8n_WEBHOOK_URL=...
```

### 4. Sanity CMS Setup
```bash
cd apps/cms
npm install -g @sanity/cli
sanity init
sanity start
```

---

## Frontend (`/apps/website`)
### 5. Scaffold Nuxt App
```bash
cd apps
npx nuxi init website
cd website
pnpm install
```

### 6. Tailwind & Supabase Setup
```bash
pnpm add -D tailwindcss postcss autoprefixer
pnpx tailwindcss init -p
pnpm add @supabase/supabase-js
```

### 7. Configure Nuxt
- Add Tailwind to `nuxt.config.ts`
- Create pages: `/`, `/about`, `/products`, `/how-it-works`, `/contact`, `/dashboard`

### 8. Supabase Auth Integration
Create:
- `composables/useSupabase.ts`
- Middleware to guard protected routes (`auth.ts`)
- `store/user.ts` (Pinia or useNuxtState)
- Auth pages: `/login`, `/signup`, handle magic link redirects

```ts
// useSupabase.ts
import { createClient } from '@supabase/supabase-js'
const supabaseUrl = useRuntimeConfig().public.supabaseUrl
const supabaseAnonKey = useRuntimeConfig().public.supabaseAnonKey
export const supabase = createClient(supabaseUrl, supabaseAnonKey)
```

---

## Backend (`/apps/backend`)
### 9. Initialize FastAPI
```bash
cd apps
mkdir backend && cd backend
python -m venv venv
source venv/bin/activate
pip install fastapi uvicorn python-dotenv httpx
```

### 10. `main.py` Example
```python
from fastapi import FastAPI, Request
from fastapi.middleware.cors import CORSMiddleware
from dotenv import load_dotenv
import os, httpx

load_dotenv()
app = FastAPI()
app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)

@app.post("/trigger-tool")
async def trigger_tool(request: Request):
    data = await request.json()
    user_id = data.get("user_id")
    tool_id = data.get("tool_id")
    input_payload = data.get("input")

    headers = {"Authorization": f"Bearer {os.getenv('OPENAI_API_KEY')}"}
    webhook_url = os.getenv("n8n_WEBHOOK_URL")

    async with httpx.AsyncClient() as client:
        res = await client.post(webhook_url, json={"user": user_id, "tool": tool_id, "input": input_payload}, headers=headers)
        return res.json()
```

---

## Supabase Setup
### 11. Tables & RLS Policies
- `users` (id, email, role)
- `tools` (id, name, description, webhook_url, plan)
- `logs` (id, user_id, tool_id, output, timestamp)

Enable RLS and policies:
```sql
-- Tools by user role
CREATE POLICY "User can access tool by role"
ON tools
FOR SELECT
USING ( auth.role() = plan OR auth.role() = 'admin' );
```

---

## Final Step
### 12. Run Apps Locally
```bash
# Website
cd apps/website
pnpm dev

# Backend
cd apps/backend
uvicorn main:app --reload

# CMS
cd apps/cms
sanity start
```

---

## ✅ You're now ready to start developing and scaling your AI-powered SaaS marketplace!
Each app has a separate README for granular documentation and deployment instructions.

**Next Up:** Connect to Stripe or LemonSqueezy, set up billing and usage tracking, and expand your n8n workflow library.

---

_This README covers scaffolding, setup, and integration for a production-ready monorepo._
