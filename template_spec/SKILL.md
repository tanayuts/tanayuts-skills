---
name: gem-template-spec
description: System Directive and Implementation Guide template for AI Coding Agents.
---

# Project: {{PROJECT_NAME}} — {{PROJECT_TAGLINE}}

**System Directive & Implementation Guide for AI Coding Agent**
**Version 1.0**

> 📝 **{{PROJECT_NAME}}** — English project name, used as an identifier throughout the document.
> Example: `StockBot`, `HealthTracker`, `ShopManager`, `TaskFlow`
>
> **{{PROJECT_TAGLINE}}** — Short tagline (3-8 words).
> Example: `AI-Powered Stock Analysis Platform`, `Personal Health Dashboard`, `Team Task Management System`

---

---

# 📖 Section 0 — How to Use This Template

This document is a **template for creating specifications** to be sent to an AI Coding Agent (Claude, Gemini, GPT, or other agents) to build a project from start to finish.

## Symbols Used in the Document

| Symbol | Meaning |
|---|---|
| `{{VARIABLE}}` | Placeholder — Replace with the actual value |
| `> 📝 ...` | Guide — Instructions on how to fill it out. Delete after completion. |
| `> ⚙️ OPTIONAL` | Optional Section — Can be deleted if not used. |
| `> ⚠️ CONDITIONAL` | Conditional Section — Depends on answers in the Questionnaire. Delete if conditions are not met. |
| `<!-- EXAMPLE -->` | Example — Delete after filling in real values. |

## How to Use

1. **Always fill out Section 1 (Questionnaire) first** — your answers will determine which sections to keep or delete.
2. **Fill out from top to bottom** — the order of the sections corresponds to the logical sequence of design; do not skip around.
3. **For sections marked as CONDITIONAL/OPTIONAL** — read the conditions. If they do not apply, delete the entire section.
4. **Once completed** — delete all guide blocks (`> 📝`) and example comments, leaving only the clean content to send to the AI agent.
5. **If the project is too large for a single conversation thread** — divide and send it phase by phase (see Section 18).

---

---

# 🎯 Section 1 — Project Questionnaire

Fill out this section first. The answers will determine decisions throughout the document.

---

## Q1. Project Information

```
Project Name:     {{PROJECT_NAME}} = ___
Tagline:          {{PROJECT_TAGLINE}} = ___
```

> 📝 English name, short and memorable. 
> If there is a local language name, put it in parentheses, e.g., `Poonsup (พูนทรัพย์)`

---

## Q2. What does this project do?

```
{{PROJECT_DESCRIPTION}} = ___
```

> 📝 Write 2-4 sentences explaining what the system does, who uses it, and what problem it solves.
>
> <!-- EXAMPLE -->
> Examples:
> - "An AI-powered stock analysis system running locally that aggregates news, analyzes sentiment, sends signals via Telegram, and executes paper trades automatically."
> - "A small clinic management app for appointments, patient histories, invoicing, and revenue dashboards."
> - "An e-commerce website for handmade goods featuring a shopping cart, Stripe payment gateway, and an order management admin dashboard."

---

## Q3. Complexity Level

Select 1 option — This affects the recommended number of builds and phases:

```
[ ] Simple   — CRUD app / landing page / small tool
               → Recommended: 1 build, 2-4 phases
[ ] Medium   — Contains business logic, integrations, auth
               → Recommended: 1-2 builds, 4-6 phases
[ ] Complex  — Multi-service, background jobs, real-time, AI
               → Recommended: 2-3 builds, 6-8+ phases
```

---

## Q4. Deployment Scale

Select 1 option — This affects Section 17 (Infrastructure):

```
[ ] Local only   — Runs on your own machine (personal tool)
[ ] LAN          — Used within a household or office (accessed via WiFi)
[ ] Cloud        — Deployed to a production server (public access)
```

---

## Q5. System Users

```
[ ] Just myself      (personal tool — auth can be optional)
[ ] Small team 2-10  (requires auth + basic roles)
[ ] Public           (requires auth + security + rate limiting)
```

> 📝 This answer affects Section 10 (Auth) — if "Just myself", the section will be marked as OPTIONAL.

---

## Q6. Your Coding Level

```
[ ] Beginner       — Little to no coding experience; requires detailed step-by-step guidance.
[ ] Intermediate   — Can code, but needs architectural guidance.
[ ] Advanced       — Just needs the spec for the agent to implement directly.
```

> 📝 Affects Section 2 — the level of detail for code comments and explanations.

---

## Q7. Language for Code Comments

```
{{CODE_COMMENT_LANGUAGE}} = ___
```

> 📝 Examples: `English`, `Thai`, `Japanese`
> Recommended: English (global standard, AI agents perform best).

---

---

# 🤖 Section 2 — Context & Role

Copy this section directly — edit only the variables in `{{...}}`.

---

You are an Expert Full-Stack Developer and System Architect. I am your Project Manager. My coding skill level is **{{SKILL_LEVEL}}**. Write complete, robust, production-ready code. Handle all error catching, edge cases, and provide step-by-step terminal commands. Do not assume I can fill in the blanks. Comment all code clearly in **{{CODE_COMMENT_LANGUAGE}}**.

> 📝 **{{SKILL_LEVEL}}** — Choose based on your Q6 answer:
> - `beginner level` → agent will write detailed comments, provide all terminal commands step-by-step, and explain concepts.
> - `intermediate level` → agent will focus on architectural decisions and explain only complex parts.
> - `advanced level` → agent will write concise code and comment only on non-obvious logic.

---

---

# 📏 Section 3 — Operational Rules

These rules are universal — applicable to any project and AI agent. Modify only the variables in `{{...}}`.

---

## BUILD SCOPE RULE

> ⚠️ CONDITIONAL — Use only if the project contains more than 1 build (Q3 = Medium/Complex).
> If only 1 build, delete this section and use the Phase Rule below.

This spec is divided into {{TOTAL_BUILDS}} builds. Do not mix them.

{{BUILD_DEFINITIONS}}

> 📝 **{{TOTAL_BUILDS}}** — Total number of builds (1-3)
>
> **{{BUILD_DEFINITIONS}}** — Details for each build using this format:
> ```
> **Build 1 — Core (implement this first):**
> - feature A
> - feature B
> - feature C
>
> **Build 2 — Extended (do NOT include in Build 1):**
> - feature D
> - feature E
> ```
>
> <!-- EXAMPLE -->
> Example (e-commerce):
> ```
> Build 1 — Core:
> - Product catalog (CRUD)
> - Shopping cart
> - Checkout with Stripe
> - Admin dashboard
>
> Build 2 — Enhanced:
> - Wishlist
> - Review & rating system
> - Email notifications
> - Analytics dashboard
> ```

When I say **"start Build 1"** → implement Build 1 only.
When I say **"start Build N"** → implement Build N items one at a time, in the order I specify.

---

## PHASE RULE

Build and verify each Phase before proceeding to the next.
Do not write code for Phase N+1 until Phase N runs without errors.
After each Phase, provide exact terminal commands to verify that Phase works.
If a Phase fails, fix it completely before moving on.

---

## AGENT INVARIANT RULES

The AI Agent must strictly adhere to the following development invariant rules:

1. **One Change, One Verify**: Every time code is added or modified (e.g., adding a function, modifying a model), you must immediately compile, run tests, or use a short script to verify the output. Do not edit multiple files without verification steps.
2. **No Ignored Errors**: If compile, lint, test, or runtime errors occur during verification, do not write any new features. Fix the error completely first.
3. **Preserve Code Quality & Comments**: Do not destroy the original code structure or delete existing important comments unless explicitly commanded to do so.

---

## PONYTAIL LAZY ARCHITECTURE RULE

Adhere to the "Ponytail Ladder" principles to limit unnecessary complexity (YAGNI):
1. **Does it need to exist?**: Avoid building abstractions (e.g., single-implementation interfaces, delegating class wrappers) or scaffolding for speculative future needs. If it's not needed today, don't write it.
2. **First Choice (Stdlib & Native First)**: Prefer standard libraries or native platform capabilities over external packages (e.g., use native HTML date pickers over third-party React date components).
3. **Banned Over-engineering**: Do not expose configuration options for static settings. Write the minimal clean code required to build a stable and secure system.

---

## CONTEXT & HANDOFF PROTOCOL

When the codebase grows and chat context boundaries are reached (or at the end of each Phase), follow this protocol:

1. **Start a new conversation**: Always start a new conversation for the next phase to clear garbage memory context.
2. **Write Handoff Summary**: Before closing the current conversation, tell the AI Agent: *"Create a Handoff Summary"*. The agent must output status in this exact format:
   ```markdown
   ### Phase [N] Handoff Status
   - **Implemented & Verified:** [List of files created/modified and verified passing]
   - **Current Blockers/Errors:** [Unresolved issues, bugs, or last known errors. If none, write "None"]
   - **Next Action for Phase [N+1]:** [The immediate first action item to start the next session]
   ```
3. **Initialize New Session**: In the new conversation, paste this entire Spec and append the latest Handoff Summary. Then instruct the agent:
   *"We are on Phase [N+1]. All previous phases are complete. Now implement Phase [N+1] only."*

---

## DEBUGGING RULE

When something breaks, I will paste the full error message exactly as-is.
Do not ask me to summarize or describe the error.
Read the full error and fix it directly.

---

---

# 📌 Section 4 — Project Overview

---

## Description

{{PROJECT_FULL_DESCRIPTION}}

> 📝 **{{PROJECT_FULL_DESCRIPTION}}** — Explain the project fully in 1 paragraph (5-10 sentences).
> Covers:
> - What the system does (what)
> - Who uses it (who)
> - How it works in general (how — high level)
> - Where it is deployed (where)
>
> <!-- EXAMPLE -->
> Example (clinic management):
> "ClinicFlow is a small clinic management system running on a Docker local network within the clinic.
> The primary users are doctors and staff. It is used to book appointments, record medical histories,
> generate invoices, and view monthly revenue dashboards. It is accessed via web browsers on PCs, tablets,
> or mobile phones within the clinic's WiFi network. The entire system is launched with a single docker-compose up -d command."

---

## Key Features

{{KEY_FEATURES}}

> 📝 **{{KEY_FEATURES}}** — List all key features as bullet points.
> Recommended: Group by module/area and include a short "Verify" step for key features so the AI Agent can verify its work.
>
> <!-- EXAMPLE -->
> ```
> **Patient Management:**
> - Patient registration with photo
>   - *Verify:* Click register patient, upload a photo, and check that the DB file path and actual saved image are correct and match.
> - Medical history timeline
> - Allergy & medication tracking
>
> **Appointments:**
> - Calendar view (day/week/month)
> - Online booking via LINE
>   - *Verify:* Simulate receiving a LINE Webhook using curl or Postman, then verify that the appointment table is updated.
> - SMS reminder 1 hour before
>
> **Billing:**
> - Auto-generate invoice from treatment
> - Print receipt
> - Monthly revenue dashboard
> ```

---

## Startup Command

The entire system runs with a single command: `{{STARTUP_COMMAND}}`

> 📝 **{{STARTUP_COMMAND}}** — System startup command.
> Examples: `docker-compose up -d`, `npm run dev`, `python main.py`
> If multiple commands are needed, list them sequentially.

---

---

# 🛠 Section 5 — Tech Stack

Fill out the table — Put "None" if the layer does not exist in the project.

---

| Layer | Your Choice | Required? |
|---|---|---|
| **Language** | {{LANGUAGE}} | Yes |
| **Backend Framework** | {{BACKEND_FRAMEWORK}} | {{YES_NO}} |
| **Frontend Framework** | {{FRONTEND_FRAMEWORK}} | {{YES_NO}} |
| **Database** | {{DATABASE}} | {{YES_NO}} |
| **ORM / DB Client** | {{ORM}} | {{YES_NO}} |
| **Cache / Message Queue** | {{CACHE_QUEUE}} | {{YES_NO}} |
| **Background Jobs** | {{BG_JOBS}} | {{YES_NO}} |
| **Auth** | {{AUTH_METHOD}} | {{YES_NO}} |
| **Containerization** | {{CONTAINER}} | {{YES_NO}} |
| **AI / LLM** | {{AI_ENGINE}} | {{YES_NO}} |
| **Notification Channel** | {{NOTIFICATION}} | {{YES_NO}} |
| **External API** | {{EXTERNAL_API}} | {{YES_NO}} |
| **CSS / Styling** | {{CSS_FRAMEWORK}} | {{YES_NO}} |
| **UI Component Library** | {{UI_LIB}} | {{YES_NO}} |
| **Real-time** | {{REALTIME}} | {{YES_NO}} |
| **Hosting / Deploy** | {{HOSTING}} | {{YES_NO}} |

> 📝 Example values you can fill in for each layer:
>
> | Layer | Examples |
> |---|---|
> | Language | `Python 3.11`, `TypeScript 5`, `Go 1.22`, `Rust`, `Java 21` |
> | Backend | `FastAPI`, `Express`, `NestJS`, `Django`, `Spring Boot`, `Gin`, `Hono` |
> | Frontend | `Next.js 14`, `Nuxt 3`, `SvelteKit`, `React SPA (Vite)`, `Vue SPA`, `None` |
> | Database | `PostgreSQL 16`, `MySQL 8`, `MongoDB 7`, `SQLite`, `Supabase`, `None` |
> | ORM | `SQLAlchemy 2.0`, `Prisma`, `Drizzle`, `TypeORM`, `GORM`, `None` |
> | Cache/Queue | `Redis`, `RabbitMQ`, `BullMQ`, `Memcached`, `None` |
> | Background Jobs | `Celery`, `BullMQ`, `cron jobs`, `Agenda`, `None` |
> | Auth | `JWT (httpOnly cookie)`, `Session`, `OAuth2`, `API Key`, `Clerk`, `None` |
> | Container | `Docker + Docker Compose`, `Podman`, `None` |
> | AI/LLM | `Google Gemini (Free)`, `OpenAI GPT-4`, `Claude API`, `Ollama (local)`, `None` |
> | Notification | `Telegram Bot`, `Slack Webhook`, `Discord Bot`, `Email (SMTP)`, `LINE Notify`, `Push (FCM)`, `None` |
> | External API | `Alpaca Trading API`, `Stripe Payments`, `Twilio SMS`, `SendGrid`, `None` — Specify name + purpose |
> | CSS | `Tailwind CSS 3`, `Vanilla CSS`, `styled-components`, `CSS Modules`, `None` |
> | UI Library | `shadcn/ui`, `MUI`, `Ant Design`, `DaisyUI`, `Radix UI`, `None` |
> | Real-time | `WebSocket (native)`, `Socket.IO`, `SSE`, `Supabase Realtime`, `None` |
> | Hosting | `Local Docker`, `Vercel`, `Railway`, `AWS EC2`, `GCP Cloud Run`, `Fly.io`, `None` |

---

### Additional Libraries & Dependencies

{{ADDITIONAL_DEPENDENCIES}}

> 📝 **{{ADDITIONAL_DEPENDENCIES}}** — Additional libraries that must be used.
> Use format:
> ```
> **Backend:**
> - library_name >= version — purpose
>
> **Frontend:**
> - library_name ^version — purpose
> ```
>
> <!-- EXAMPLE -->
> ```
> **Backend (requirements.txt):**
> - yfinance >= 0.2.40 — stock data fetching
> - pandas >= 2.2.0 — data manipulation
> - python-telegram-bot >= 21.0 — Telegram integration
>
> **Frontend (package.json):**
> - recharts ^2.12 — chart components
> - date-fns ^3 — date formatting
> - lucide-react ^0.400 — icon library
> ```

---

### Architecture Decision Records

{{ADR_LIST}}

> 📝 **{{ADR_LIST}}** — Record why you chose this tech/pattern so the agent understands the context.
> Format per item:
> ```
> **ADR-1: Why did we use {{TECH}}**
> - Problem: {{PROBLEM}}
> - Alternatives considered: {{ALTERNATIVES}}
> - Reason for choosing: {{REASON}}
> ```
>
> <!-- EXAMPLE -->
> ```
> **ADR-1: Why FastAPI instead of Django**
> - Problem: Need async support for WebSocket + Celery
> - Alternatives: Django, Flask, FastAPI
> - Reason: FastAPI has native async, auto-docs, and better type hints
>
> **ADR-2: Why Redis as both cache and message broker**
> - Problem: Need Celery broker + WebSocket bridge
> - Alternatives: Redis, RabbitMQ, Redis + RabbitMQ separate
> - Reason: Reduces service count, Redis can do both
> ```
>
> ⚙️ OPTIONAL — Can be deleted if not needed, but highly recommended as it helps the agent align decisions with your intent.

---

---

# 📐 Section 6 — System Architecture

---

## Architecture Diagram

Draw an ASCII diagram showing which services communicate with whom and through what:

```
{{ARCHITECTURE_DIAGRAM}}
```

> 📝 **{{ARCHITECTURE_DIAGRAM}}** — ASCII art representing services and connections.
> Symbols to use:
> - `──▶` = HTTP/REST
> - `◀──▶` = bidirectional (WebSocket)
> - `- - ▶` = async (queue/pub-sub)
>
> <!-- EXAMPLE -->
> ```
> docker-compose up -d
>         │
>         ├── 🐘 PostgreSQL (port 5432)
>         │       └── app_database
>         │
>         ├── 🔴 Redis (port 6379)
>         │       └── task queue + pub/sub
>         │
>         ├── 🐍 Backend API (port 8000)
>         │       ├── REST API: /api/v1/...
>         │       ├── WebSocket: /ws/live
>         │       └── Auth: /api/v1/auth/...
>         │
>         ├── 🌿 Background Worker
>         │       └── scheduled + manual tasks
>         │
>         └── ⚛️  Frontend (port 3000)
>                 └── dashboard pages
> ```

---

## Service Communication Map

{{SERVICE_COMMUNICATION}}

> 📝 **{{SERVICE_COMMUNICATION}}** — Explain which service talks to which via what protocol.
> Format:
> ```
> - Frontend ↔ Backend: HTTP REST + WebSocket
> - Backend ↔ Database: ORM (SQLAlchemy / Prisma / etc.)
> - Backend ↔ Cache: direct connection (Redis client)
> - Worker ↔ Database: direct ORM
> - Worker ↔ Queue: task dispatch + results
> ```

---

---

# 📂 Section 7 — Directory Structure

---

```text
{{DIRECTORY_TREE}}
```

> 📝 **{{DIRECTORY_TREE}}** — All files the agent needs to create; include comments explaining their purpose.
> Recommended: Group by service (backend / frontend / infra)
>
> <!-- EXAMPLE -->
> ```text
> myproject/
> ├── docker-compose.yml              # Orchestrates all services
> ├── .env                            # API keys & secrets (never commit)
> ├── .gitignore
> │
> ├── backend/
> │   ├── Dockerfile
> │   ├── requirements.txt
> │   ├── app/
> │   │   ├── main.py                 # App entry point
> │   │   ├── config.py               # Load .env, export constants
> │   │   ├── database.py             # DB engine + session
> │   │   ├── models/                 # ORM models
> │   │   ├── schemas/                # Request/Response schemas
> │   │   ├── routers/                # API route handlers
> │   │   ├── services/               # Business logic
> │   │   └── utils/                  # Shared utilities
> │   └── worker/
> │       ├── app.py                  # Worker instance
> │       └── tasks.py                # Background task definitions
> │
> ├── frontend/
> │   ├── Dockerfile
> │   ├── package.json
> │   └── src/
> │       ├── app/                    # Pages (routes)
> │       ├── components/             # Reusable UI components
> │       ├── lib/                    # API client, utilities
> │       └── hooks/                  # Custom React hooks
> │
> └── logs/
>     └── .gitkeep
> ```
>
> Rule: **List every single file** — do not use `...` or "etc." The agent must know exactly which files to create.

---

---

# 🗄 Section 8 — Data Models

Copy the "Model Blueprint" below for every model in the project.

---

## Model Blueprint Format

Use this format for every model:

---

### Model: `{{MODEL_NAME}}`

> 📝 Briefly explain what data this model stores.

| Column | Type | Constraint | Note |
|---|---|---|---|
| {{COLUMN_NAME}} | {{COLUMN_TYPE}} | {{CONSTRAINT}} | {{NOTE}} |

**Relationships:**
- {{RELATIONSHIP}}

**Default Data (seed on first run):**
{{SEED_DATA}}

**Business Rules:**
- {{RULE}}

---

> 📝 **Field descriptions:**
>
> **Commonly used Column Types:**
> | Type | Use Case | Example |
> |---|---|---|
> | `Integer PK` | Primary key auto-increment | `id` |
> | `String` | Short text | `username`, `email`, `status` |
> | `String UNIQUE` | Uniqueness constraint | `email`, `slug`, `api_key` |
> | `Text` | Long text | `description`, `content`, `notes` |
> | `Float` | Decimal numbers | `price`, `percentage`, `score` |
> | `Integer` | Integer numbers | `quantity`, `count`, `age` |
> | `Boolean` | true/false | `is_active`, `is_verified` |
> | `DateTime` | Date + time | `created_at`, `expires_at` |
> | `JSONB` | JSON object | `metadata`, `settings`, `tags` |
> | `Integer FK` | Foreign key | `user_id → users.id` |
>
> **Commonly used Constraints:**
> - `NOT NULL` — Cannot be empty
> - `nullable` — Can be empty
> - `DEFAULT value` — Default value when not specified
> - `UNIQUE` — Cannot be duplicate
>
> **Relationships:**
> - `User has many Orders` (1:N)
> - `Order belongs to User` (N:1)
> - `Product has many Tags through ProductTag` (M:N)
>
> **Business Rules — Examples:**
> - "If `allocation_pct > 0` use this value, otherwise use the global default value from settings."
> - "status must be one of: PENDING, APPROVED, REJECTED, EXPIRED."
> - "Seed default admin user from .env if no users exist in the system."

---

## Your Data Models

{{DATA_MODELS}}

> 📝 **{{DATA_MODELS}}** — Copy the Model Blueprint format above and paste 1 block per model.
> Include all models the project requires.
>
> <!-- EXAMPLE -->
> ```
> ### Model: `User`
> | Column | Type | Constraint | Note |
> |---|---|---|---|
> | id | Integer PK | NOT NULL | Auto-increment |
> | email | String UNIQUE | NOT NULL | Login email |
> | hashed_password | String | NOT NULL | bcrypt — never store plaintext |
> | role | String | DEFAULT 'user' | 'admin' or 'user' |
> | is_active | Boolean | DEFAULT True | Soft delete |
> | created_at | DateTime | NOT NULL | Auto-set on create |
>
> **Seed:** Create admin user from .env (ADMIN_EMAIL, ADMIN_PASSWORD)
>
> ### Model: `Product`
> | Column | Type | Constraint | Note |
> |---|---|---|---|
> | id | Integer PK | NOT NULL | Auto-increment |
> | name | String | NOT NULL | Product display name |
> | price | Float | NOT NULL | In THB |
> | stock | Integer | DEFAULT 0 | Current stock count |
> | category_id | Integer FK | nullable | → categories.id |
> | created_at | DateTime | NOT NULL | |
>
> **Rules:** stock must not go negative — check before ordering.
> ```

---

---

# ⚙️ Section 9 — Configuration & Database Setup

---

### Config Pattern

{{CONFIG_DESCRIPTION}}

> 📝 **{{CONFIG_DESCRIPTION}}** — Explain the config pattern used.
> Core Rule: **Load .env once. All other modules import from config — never call env reader elsewhere.**
>
> <!-- EXAMPLE -->
> ```python
> # app/config.py
> from dotenv import load_dotenv
> import os
>
> load_dotenv()
>
> DATABASE_URL = os.getenv("DATABASE_URL")
> REDIS_URL    = os.getenv("REDIS_URL", "redis://localhost:6379/0")
> SECRET_KEY   = os.getenv("SECRET_KEY")
> # ... all other env vars
> ```
>
> Specify every environment variable the project needs, grouped by service.

---

### Database Setup

{{DATABASE_SETUP}}

> 📝 **{{DATABASE_SETUP}}** — Explain the database connection setup.
> Includes: engine creation, session factory, migration tool (if used).
>
> ⚠️ CONDITIONAL — Delete if Tech Stack does not include a Database.

---

---

# 🔐 Section 10 — Auth Implementation

> ⚠️ CONDITIONAL — Use only if Q5 ≠ "Just myself" or Q6 requires auth.
> If auth is not needed, delete this entire section.

---

### Auth Endpoints

{{AUTH_ENDPOINTS}}

> 📝 **{{AUTH_ENDPOINTS}}** — Define all auth endpoints.
> Use format:
> ```
> **POST /api/v1/auth/login**
> - Body: { "username": str, "password": str }
> - Response: token/session return method
> - Logic:
>   1. Verify password
>   2. Generate token
>   3. Return / set cookie
>
> **GET /api/v1/auth/me** (protected)
> - Response: { current user info }
>
> **POST /api/v1/auth/logout** (protected)
> - Response: clear token/session
> ```

---

### Auth Middleware

{{AUTH_MIDDLEWARE}}

> 📝 **{{AUTH_MIDDLEWARE}}** — Explain how to protect routes.
> Specify:
> - Where the token is located (cookie / header / query param)
> - Verification method
> - Behavior on expired / invalid token
> - Which routes are public (no auth required)

---

---

# 🔌 Section 11 — API Endpoints

---

## Endpoint Format

Use this format for every endpoint:

```
**{{METHOD}} {{PATH}}** {{AUTH_BADGE}}
- Body: { {{REQUEST_BODY}} }
- Query Params: {{QUERY_PARAMS}}
- Response: { {{RESPONSE_BODY}} }
- Logic:
  1. {{STEP}}
  2. {{STEP}}
- Verification:
  - {{VERIFY_INSTRUCTIONS_AND_COMMANDS}}
- Error Cases:
  - {{ERROR_CODE}}: {{DESCRIPTION}}
```

> 📝 **{{AUTH_BADGE}}** — Mark `*(protected)*` if auth is required, empty if public.
>
> **{{METHOD}}** — `GET`, `POST`, `PUT`, `PATCH`, `DELETE`
>
> **{{VERIFY_INSTRUCTIONS_AND_COMMANDS}}** — Testing commands/instructions for this endpoint for the AI Agent to run and verify (e.g., `curl` command, small script, or expected return values).

---

## Your Endpoints

{{API_ENDPOINTS}}

> 📝 **{{API_ENDPOINTS}}** — List all endpoints, grouped by router/resource.
>
> <!-- EXAMPLE -->
> ```
> ### Products Router (prefix: /api/v1/products)
>
> **GET /api/v1/products** *(protected)*
> - Query: page, limit, category, search
> - Response: { "items": [...], "total": int, "page": int }
> - Logic:
>   1. Apply filters from query params
>   2. Paginate results
>   3. Return with total count
> - Verification:
>   - `curl -H "Authorization: Bearer <token>" "http://localhost:8000/api/v1/products?page=1&limit=10"` (Expects HTTP 200 and list of items)
>
> **POST /api/v1/products** *(protected, admin only)*
> - Body: { "name": str, "price": float, "stock": int, "category_id": int? }
> - Response: { created product }
> - Logic:
>   1. Validate body
>   2. Insert to DB
>   3. Return created product
> - Verification:
>   - `curl -X POST -H "Content-Type: application/json" -H "Authorization: Bearer <token>" -d '{"name": "Apple", "price": 10.0, "stock": 5}' http://localhost:8000/api/v1/products` (Expects HTTP 201 and created product object)
> - Errors:
>   - 400: Invalid input
>   - 409: Product name already exists
> ```
>
> Rule: **List every single endpoint** — do not miss any.

---

---

# 🧠 Section 12 — Core Processing Pipeline

> ⚠️ CONDITIONAL — Use only if the project contains a complex processing pipeline or business logic chain.
> Examples of when to include this section: AI analysis chain, order processing, data ETL, image pipeline.
> If the project is a simple CRUD application, delete this section.

---

### Pipeline Architecture

{{PIPELINE_ARCHITECTURE}}

> 📝 **{{PIPELINE_ARCHITECTURE}}** — Flow of the pipeline:
> ```
> [Input]
>     │
>     ▼
> Step 1 — {{STEP_NAME}}
>     │  Output: {{OUTPUT}}
>     ▼
> Step 2 — {{STEP_NAME}}
>     │  Input: {{INPUT}}
>     │  Output: {{OUTPUT}}
>     ▼
> Step 3 — {{STEP_NAME}}
>     ▼
> [Gate / Decision]
>     ├─ PASS → {{ACTION}}
>     └─ FAIL → {{ACTION}}
> ```
>
> For each step specify:
> - System prompt / logic (if it is an AI agent)
> - Input → Output format
> - Error handling (what to do if this step fails)

---

### Rate Limiting & Retry

{{RATE_LIMIT_STRATEGY}}

> 📝 **{{RATE_LIMIT_STRATEGY}}** — Explain:
> - Delay between API calls (e.g., `time.sleep(5)`)
> - Retry policy on error (number of retries, wait time)
> - Behavior when quota is exhausted
>
> <!-- EXAMPLE -->
> ```
> - 5 seconds between AI agent calls within same item
> - 15 seconds between items
> - On API error: wait 30s, retry once. If retry fails: log, skip item, continue
> - On 429 quota error: send alert notification immediately
> ```

---

### Output Parsing

{{OUTPUT_PARSING}}

> 📝 **{{OUTPUT_PARSING}}** — If pipeline output is unstructured text that must be parsed (e.g., JSON from LLM):
> - Strip markdown fences before parsing
> - Wrap in try-except
> - If malformed: log error, skip item, do not crash

---

---

# 🔗 Section 13 — External Integrations

> ⚠️ CONDITIONAL — Use only if the Tech Stack includes Notification / External API.
> Delete unused sub-sections.

---

## Integration Format

Use this format for every integration:

---

### Integration: {{SERVICE_NAME}}

> 📝 Explain what it is used for.

- **SDK / Library:** {{LIBRARY}}
- **Auth:** {{API_KEY_OR_OAUTH}}
- **Safety:** {{SAFETY_NOTES}}

**Key Functions:**

{{INTEGRATION_FUNCTIONS}}

> 📝 **{{INTEGRATION_FUNCTIONS}}** — List the key functions that need to be implemented.
> Format per function:
> ```
> **Function: `function_name(params) → return_type`**
> 1. Step 1
> 2. Step 2
> 3. On error: handle
> ```
>
> <!-- EXAMPLE -->
> ```
> **Function: `execute_trade(ticker, action, quantity) → bool`**
> 1. Fetch account balance
> 2. Calculate position size
> 3. Submit order
> 4. On success: save to DB, return True
> 5. On error: log, return False
> ```

**Message Formats:**

{{MESSAGE_FORMATS}}

> 📝 **{{MESSAGE_FORMATS}}** — If the integration sends messages (notifications), specify the full format.
> Applicable to: Telegram, Slack, Discord, Email, LINE, SMS.
>
> <!-- EXAMPLE -->
> ```
> **Signal Alert:**
> 📌 Ticker: NVDA
> 📊 Action: 🟢 BUY
> 💯 Confidence: 82.5%
> [Approve] [Reject]    ← inline buttons
>
> **Daily Summary:**
> 📊 Daily Report — 2025-01-15
> Processed: 9 items
> Generated: 3 signals
> Total P&L: +$142.50
>
> **Error Alert:**
> 🚨 System Error
> Pipeline failed at 22:30
> Error: [message]
> ```

---

## Your Integrations

{{INTEGRATIONS_LIST}}

> 📝 **{{INTEGRATIONS_LIST}}** — Copy the Integration Format above and paste 1 block per integration.
> Group by type:
> - **Notification Channel** — Telegram / Slack / Email / LINE / etc.
> - **External Action API** — trading / payment / shipping / etc.
> - **Data Source** — scraper / RSS / webhook / third-party API

---

---

# 🔄 Section 14 — Background Jobs & Scheduling

> ⚠️ CONDITIONAL — Use only if the Tech Stack includes Background Jobs.
> If none, delete this section.

---

### Worker Setup

{{WORKER_SETUP}}

> 📝 **{{WORKER_SETUP}}** — Explain worker config.
> Covers: worker library, broker URL, timezone, startup command.
>
> <!-- EXAMPLE -->
> ```python
> celery_app = Celery("myapp", broker=REDIS_URL, backend=REDIS_URL)
> celery_app.conf.timezone = "Asia/Bangkok"
> celery_app.conf.enable_utc = True
> ```

---

### Task Definitions

{{TASK_DEFINITIONS}}

> 📝 **{{TASK_DEFINITIONS}}** — Define every task using this format:
> ```
> **Task: `task_name`**
> - Trigger: scheduled / manual / event
> - Logic:
>   1. Step 1
>   2. Step 2
>   3. On error: handle
> - Broadcasts: {{ WebSocket events if any }}
> ```

---

### Schedule (Cron Jobs)

{{SCHEDULE_TABLE}}

> 📝 **{{SCHEDULE_TABLE}}** — Table of scheduled jobs.
> ```python
> beat_schedule = {
>     "job-name": {
>         "task": "task_name",
>         "schedule": crontab(hour=8, minute=0),
>     },
> }
> ```
>
> Specify timezone, run days (e.g., weekdays only?), and frequency.

---

### Cross-Service Communication

{{CROSS_SERVICE_PATTERN}}

> 📝 **{{CROSS_SERVICE_PATTERN}}** — If the worker needs to send data to another service (e.g., Worker → WebSocket), explain the pattern used.
>
> <!-- EXAMPLE -->
> Problem: Worker and API Server run in different processes/containers — cannot share memory.
> Solution: Use Redis Pub/Sub as a bridge:
> ```
> Worker → redis.publish('events', json) → API Server subscriber → WebSocket broadcast
> ```
>
> ⚙️ OPTIONAL — Delete if the worker does not need to communicate with other services.

---

---

# 📡 Section 15 — Real-time & WebSocket

> ⚠️ CONDITIONAL — Use only if the Tech Stack includes Real-time (WebSocket / SSE / etc.).
> If none, delete this section.

---

### WebSocket Endpoint

{{WS_ENDPOINT}}

> 📝 **{{WS_ENDPOINT}}** — Explain WebSocket endpoint:
> - Path (e.g., `/ws/live`)
> - Auth method on connect
> - Reconnect strategy
>
> <!-- EXAMPLE -->
> ```
> WS /ws/live
> - Auth: read JWT from httpOnly cookie on upgrade handshake
> - If invalid: close with code 1008
> - Client auto-reconnects every 3 seconds on disconnect
> ```

---

### Event Types

{{WS_EVENTS}}

> 📝 **{{WS_EVENTS}}** — List all events sent via WebSocket:
> ```
> | Event Type | Payload | Trigger |
> |---|---|---|
> | SIGNAL_NEW | { signal data } | New signal created |
> | PIPELINE_STARTED | { run_type } | Pipeline begins |
> | SYSTEM_STATUS | { is_healthy } | Every 30 seconds |
> ```

---

---

# ⚛️ Section 16 — Frontend Implementation

> ⚠️ CONDITIONAL — Use only if the Tech Stack includes Frontend Framework.
> If no frontend, delete this section.

---

## Design System

### Visual Style

{{DESIGN_STYLE}}

> 📝 **{{DESIGN_STYLE}}** — Explain the desired visual style.
> Example: "Linear/Vercel — clean, minimal, modern, dark mode"
> Or: "Warm, friendly, rounded corners, light mode with pastel colors"

### Color Palette

{{COLOR_PALETTE}}

> 📝 **{{COLOR_PALETTE}}** — Define key colors:
> ```
> Background:    #hex  (description)
> Surface:       #hex  (card/panel background)
> Border:        #hex  (dividers)
> Text primary:  #hex
> Text muted:    #hex
> Accent:        #hex  (actions, links, active states)
> Success:       #hex  (positive actions)
> Danger:        #hex  (negative/destructive)
> Warning:       #hex  (caution, pending)
> ```

### Typography

{{TYPOGRAPHY}}

> 📝 **{{TYPOGRAPHY}}** — Font family to use.
> Example: `Inter (Google Fonts)`, `Roboto`, `Noto Sans Thai`, `system-ui`

---

## Auth Flow (Frontend)

{{FRONTEND_AUTH_FLOW}}

> 📝 **{{FRONTEND_AUTH_FLOW}}** — Explain:
> - Login page behavior
> - How to store/send token (cookie auto-send / localStorage / etc.)
> - Session check flow (call /auth/me on page load)
> - Redirect behavior on HTTP 401
>
> ⚠️ CONDITIONAL — Delete if auth is not used.

---

## Layout

### Desktop

{{DESKTOP_LAYOUT}}

> 📝 **{{DESKTOP_LAYOUT}}** — Explain the layout for large screens (≥768px).
> Example: "Fixed left sidebar 240px + main content area"

### Mobile

{{MOBILE_LAYOUT}}

> 📝 **{{MOBILE_LAYOUT}}** — Explain the layout for mobile screens (<768px).
> Example: "Bottom navigation bar with 5 icons, no sidebar"
> Or: "Hamburger menu → slide-out drawer"

### Navigation Items

{{NAV_ITEMS}}

> 📝 **{{NAV_ITEMS}}** — List menu items:
> ```
> 🏠 Overview      → /dashboard
> 📊 Portfolio      → /dashboard/portfolio
> 📋 History        → /dashboard/history
> ⚙️ Settings       → /dashboard/settings
> ```

---

## Page Specs

Use this format for every page:

---

### Page: {{PAGE_ICON}} {{PAGE_NAME}} (`{{ROUTE}}`)

{{PAGE_DESCRIPTION}}

> 📝 Explain what this page displays and what actions the user can perform.

**Components:**

{{PAGE_COMPONENTS}}

> 📝 **{{PAGE_COMPONENTS}}** — List components on this page:
> ```
> - MetricCard (2x2 grid mobile, 4-col desktop)
>   | Metric | Calculation |
>   | Total Value | sum of X |
>   | Win Rate | X / Y × 100 |
>
> - DataTable
>   | Column | Description |
>   | Name | clickable link |
>   | Status | colored badge |
>
> - ActionButton: "Run Process" → POST /api/v1/run → disable while running, show spinner
> ```

**Data Source:**
- API: `{{ENDPOINT}}`
- Real-time: `{{WS_EVENT}}` (if any)

**Interactions:**

{{PAGE_INTERACTIONS}}

> 📝 **{{PAGE_INTERACTIONS}}** — User action → system response:
> ```
> - Click "Run" → call API → disable button + spinner → listen for completion event → re-enable
> - Change filter → re-fetch data with new params
> - Click row → navigate to detail page
> ```

---

## Your Pages

{{PAGE_SPECS}}

> 📝 **{{PAGE_SPECS}}** — Copy the Page Format above and paste 1 block per page.
> Include all pages required for the project.

---

---

# 🐳 Section 17 — Infrastructure

> ⚠️ CONDITIONAL — Use only if the Tech Stack includes Container / Docker.
> If Docker is not used, delete the docker-compose and Dockerfile sections, but keep the .env and .gitignore sections.

---

## docker-compose.yml

{{DOCKER_COMPOSE}}

> 📝 **{{DOCKER_COMPOSE}}** — Full docker-compose.yml file.
> Specify all elements:
> - All services (database, cache, backend, worker, frontend, etc.)
> - Ports mapping
> - Env_file
> - Volumes
> - Healthcheck
> - Depends_on with conditions
> - Restart policy
>
> <!-- EXAMPLE -->
> ```yaml
> version: '3.9'
> services:
>   postgres:
>     image: postgres:16-alpine
>     environment:
>       POSTGRES_DB: myapp
>       POSTGRES_USER: ${POSTGRES_USER}
>       POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
>     volumes:
>       - postgres_data:/var/lib/postgresql/data
>     healthcheck:
>       test: ["CMD-SHELL", "pg_isready -U ${POSTGRES_USER}"]
>       interval: 10s
>     restart: unless-stopped
>
>   backend:
>     build: ./backend
>     ports:
>       - "8000:8000"
>     env_file: .env
>     depends_on:
>       postgres:
>         condition: service_healthy
>     restart: unless-stopped
>
> volumes:
>   postgres_data:
> ```

---

## Dockerfiles

{{DOCKERFILES}}

> 📝 **{{DOCKERFILES}}** — Dockerfile for each service that needs to be built.
> Provide the full file with explanatory comments.

---

## .env Template

```env
{{ENV_TEMPLATE}}
```

> 📝 **{{ENV_TEMPLATE}}** — All environment variables that need to be configured.
> Group them using comments:
> ```env
> # ── Database ─────────────────────────────
> POSTGRES_USER=myapp
> POSTGRES_PASSWORD=your_password_here
> DATABASE_URL=postgresql+psycopg2://myapp:your_password_here@postgres:5432/myapp
>
> # ── Auth ─────────────────────────────────
> SECRET_KEY=generate_random_string_here
> ADMIN_USERNAME=admin
> ADMIN_PASSWORD=your_password_here
>
> # ── External APIs ───────────────────────
> API_KEY=your_api_key_here
> ```
>
> Rule: **Include every single variable the system needs. Do not make the agent guess.**

---

## .gitignore

{{GITIGNORE}}

> 📝 **{{GITIGNORE}}** — Files that should not be committed.
> Must include at least: `.env`, `node_modules/`, `__pycache__/`, `*.pyc`, `.next/`, `logs/`

---

## Dependencies

{{DEPENDENCIES}}

> 📝 **{{DEPENDENCIES}}** — The complete dependency file (requirements.txt / package.json / go.mod / etc.).
> Include version ranges and comments stating their purpose.
>
> <!-- EXAMPLE -->
> ```txt
> # Web Framework
> fastapi>=0.115.0,<1.0.0
> uvicorn[standard]>=0.32.0
>
> # Database
> sqlalchemy>=2.0.0,<3.0.0
> psycopg2-binary>=2.9.0
>
> # Auth
> python-jose[cryptography]>=3.3.0
> passlib[bcrypt]>=1.7.4
> ```

---

---

# 🚀 Section 18 — Execution Phases

Execute strictly in this order. Write all code before asking for confirmation. Present one file at a time.

---

## Phase Table

{{PHASE_TABLE}}

> 📝 **{{PHASE_TABLE}}** — Table of all execution phases to be completed.
> Format:
> ```
> | Phase | Files | Description |
> |---|---|---|
> | Phase 1 | .env, docker-compose.yml, Dockerfiles | Infrastructure setup |
> | Phase 2 | config, database, models, migrations | Data layer |
> | Phase 3 | auth service, auth router, schemas | Authentication |
> | Phase 4 | business logic services | Core pipeline |
> | Phase 5 | remaining routers, worker tasks | API + Background jobs |
> | Phase 6 | all frontend pages + components | Dashboard |
> | Phase 7 | terminal commands only | Run & verify everything |
> ```
>
> Recommended number of phases based on complexity:
> - Simple: 2-4 phases
> - Medium: 4-6 phases
> - Complex: 6-8+ phases
>
> **Principles of separation:**
> 1. Infrastructure setup first (Docker, .env)
> 2. Data layer next (DB, models)
> 3. Backend services based on dependency order
> 4. Frontend after backend is operational
> 5. Integration & verification last

---

## First-Time Setup Commands

{{SETUP_COMMANDS}}

> 📝 **{{SETUP_COMMANDS}}** — Terminal commands to run for the first time step-by-step.
> Includes:
> - Install prerequisites (Docker, Node, Python, etc.)
> - Create project folder
> - Configure .env
> - Build & start services
> - Verify everything works
>
> <!-- EXAMPLE -->
> ```bash
> # 1. Clone/create project
> mkdir myproject && cd myproject
>
> # 2. Configure environment
> cp .env.example .env
> # → edit .env with your API keys
>
> # 3. Build and start
> docker-compose up -d
>
> # 4. Verify
> docker-compose ps            # all services should be "Up"
> curl http://localhost:8000    # backend health check
> # Open http://localhost:3000  # frontend
> ```

---

## Daily Use Commands

{{DAILY_COMMANDS}}

> 📝 **{{DAILY_COMMANDS}}** — Commands used regularly.
>
> <!-- EXAMPLE -->
> ```bash
> docker-compose up -d        # Start
> docker-compose down         # Stop
> docker-compose logs -f      # View live logs
> docker-compose restart backend  # Restart single service
> ```

---

---

# ✅ Section 19 — Constraints Checklist

Fill in every checklist item that the agent must adhere to. The agent will use this to verify code before delivering it.

---

## How to Write Constraints

> 📝 Write as a checklist using `- [ ]` grouped by category.
> Every item must be clear and testable — the agent (or you) must be able to verify whether it passes.
>
> Example of a good constraint: "JWT stored in httpOnly cookie only — never in localStorage"
> Example of a bad constraint: "Security should be good" (not testable)

---

### 🏗 Infrastructure
{{INFRA_CONSTRAINTS}}

> 📝 Examples:
> ```
> - [ ] All services start with `docker-compose up -d` — no manual steps after .env setup
> - [ ] PostgreSQL data volume persists across down/up cycles
> - [ ] All ports accessible from LAN (if deploying on LAN)
> ```

### 🔐 Security & Auth
{{SECURITY_CONSTRAINTS}}

> 📝 Examples:
> ```
> - [ ] JWT auth required on all API routes except POST /auth/login
> - [ ] Passwords hashed with bcrypt — never stored as plaintext
> - [ ] .env never committed to git (verified by .gitignore + pre-commit hook)
> - [ ] API keys validated on first startup
> ```

### 📊 Data Integrity
{{DATA_CONSTRAINTS}}

> 📝 Examples:
> ```
> - [ ] Stock quantity never goes negative — check before order
> - [ ] Unique constraints enforced at DB level, not just application level
> - [ ] All monetary values stored as Float, displayed with 2 decimal places
> ```

### 🎨 Frontend / UX
{{FRONTEND_CONSTRAINTS}}

> 📝 Examples:
> ```
> - [ ] Mobile layout uses bottom navigation (not sidebar)
> - [ ] All interactive elements have loading states
> - [ ] Dark mode is default
> - [ ] Dashboard accessible from LAN devices
> ```
>
> ⚠️ CONDITIONAL — Delete if no frontend is used.

### 🔄 Background Jobs
{{JOBS_CONSTRAINTS}}

> 📝 Examples:
> ```
> - [ ] Worker never crashes on single-item failure — catch, log, continue
> - [ ] All scheduled jobs run in correct timezone
> - [ ] Worker → WebSocket bridge uses Redis pub/sub (not direct call)
> ```
>
> ⚠️ CONDITIONAL — Delete if no background jobs are used.

### 🔗 Integrations
{{INTEGRATION_CONSTRAINTS}}

> 📝 Examples:
> ```
> - [ ] Rate limits respected — enforced sleep between API calls
> - [ ] API errors never crash the system — catch, log, notify
> - [ ] Paper/sandbox mode enforced — never hits production endpoints
> ```
>
> ⚠️ CONDITIONAL — Delete if no external integrations are used.

### 📝 Code Quality
{{CODE_QUALITY_CONSTRAINTS}}

> 📝 Examples:
> ```
> - [ ] config.py is single source for env variables — no os.getenv() elsewhere
> - [ ] Logger used everywhere — no bare print() for operational logs
> - [ ] Every module has try-except with clear error messages
> - [ ] All code is clean, modular, and commented in {{CODE_COMMENT_LANGUAGE}}
> ```

### 🔧 Custom Constraints
{{CUSTOM_CONSTRAINTS}}

> 📝 Project-specific constraints that do not fit the categories above.
> Unlimited entries allowed.

---

---

# 🐛 Section 20 — Known Gotchas & Architecture Notes

This section is for recording issues the agent must watch out for — especially cross-service communication and common mistakes.

---

## Gotcha Format

Use this format:

```
### GOTCHA {{N}} — {{SEVERITY}}: {{TITLE}}
**Problem:** {{PROBLEM_DESCRIPTION}}
**Solution:** {{SOLUTION}}
```

> 📝 **{{SEVERITY}}** — `Critical`, `Warning`, `Info`

<!-- EXAMPLE -->
```
### GOTCHA 1 — Critical: Background Worker cannot send WebSocket messages across processes
**Problem:** Worker and API Server run in different containers — broadcast from worker does not reach the client.
**Solution:** Use Redis Pub/Sub as a bridge: Worker → publish → API Server subscribe → broadcast

### GOTCHA 2 — Warning: Dockerfile missing static assets
**Problem:** Multi-stage build did not copy the public/ folder to the runner stage — assets 404
**Solution:** Add COPY --from=builder /app/public public in the runner stage

### GOTCHA 3 — Info: API rate limit of third-party service
**Problem:** Free tier limits 100 calls/day — if exceeded, returns HTTP 429
**Solution:** Implement sleep + retry logic + send an alert when the quota is close to running out
```

---

## Your Gotchas

{{GOTCHAS_LIST}}

> 📝 **{{GOTCHAS_LIST}}** — Record every issue that you know might cause problems.
> If there are none at the start, leave it blank and add them when you encounter bugs during the build.
> This section will serve as a "living document" updated continuously throughout the build process.

---

---

# 📋 Summary

```
Project:  {{PROJECT_NAME}}
Version:  {{SPEC_VERSION}}
Builds:   {{TOTAL_BUILDS}}
Phases:   {{TOTAL_PHASES}}
```

> 📝 When finished filling out:
> 1. Delete all guide blocks (`> 📝`).
> 2. Delete all `<!-- EXAMPLE -->` blocks.
> 3. Delete unused CONDITIONAL/OPTIONAL sections.
> 4. Send this entire document to the AI agent.
> 5. Tell the agent: "We are on Phase 1. Now implement Phase 1 only."
