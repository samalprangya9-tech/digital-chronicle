# The Digital Chronicle: Product Requirements Document (PRD)

**Modern Newspaper Web Platform**

| Document Info | Details |
| --- | --- |
| Document Type | Product Requirements Document |
| Version | 1.0 (Initial Release) |
| Date | September 18, 2026 |
| Status | Draft, Under Review |
| Frontend Stack | React.js + Animations |
| Backend Stack | Node.js (Express) |
| Database | PostgreSQL + Prisma ORM |
| Architecture | MERN-variant (PERN) |

## Table of Contents

1. [Project Requirements Document](#1-project-requirements-document-prd)
2. [Technical Architecture](#2-technical-architecture)
3. [System Workflow](#3-system-workflow)
4. [Security Architecture](#4-security-architecture)
5. [Technology Stack](#5-technology-stack)

---

## 1. Project Requirements Document (PRD)

### 1.1 Project Overview

> **Project Name: The Digital Chronicle**
> A full-stack, animated newspaper-style web platform that automatically collects, curates, and displays original, verified news content sourced from the internet, presented with a bold blue-and-white design, hero banners, real-time updates, and a rich editorial experience.

The platform replicates the premium feel of a modern digital newspaper, combining the visual authority of print media with real-time internet-sourced content. Its React.js frontend delivers smooth page animations, a striking navy-and-blue color palette, a full-screen hero banner on the homepage, and intuitive category navigation. The Node.js + PostgreSQL backend powers content ingestion, editorial control, and secure user management.

### 1.2 Goals & Objectives

**Primary Goals**

- Build a visually stunning newspaper website with animated UI and blue/white branding
- Automatically collect and display only original, verified news from internet sources
- Deliver a fast, accessible, mobile-responsive news reading experience
- Provide admin tools for editorial control, scheduling, and content moderation
- Ensure robust security, scalability, and performance from day one

**Success Metrics**

| KPI | Target | Timeline |
| --- | --- | --- |
| Page Load Time | < 2.0 seconds | Launch |
| Core Web Vitals (LCP) | < 2.5 seconds | Launch |
| Mobile Responsiveness | 100% coverage | Launch |
| Content Freshness | Every 15 min | Launch |
| Uptime SLA | 99.9% | Ongoing |
| Articles / Day | 50 – 200 | 30 days |

### 1.3 Target Audience

| Segment | Persona | Primary Need |
| --- | --- | --- |
| General Public | Daily News Reader | Quick, reliable news summaries |
| Professionals | Business / Tech Executive | Industry-specific deep dives |
| Students | Research-oriented User | Verified, citable sources |
| Casual Readers | Mobile Browse User | Visual appeal, easy navigation |
| Content Editors | Internal Admin | Dashboard for CMS & scheduling |

### 1.4 Feature Specifications

**Homepage / Banner Features**

- Full-viewport hero banner with animated breaking-news ticker
  - Auto-rotating top-3 featured stories with parallax fade effect
  - Animated gradient overlay: deep navy → transparent
  - Countdown/live badge for breaking news articles
- Category navigation bar (sticky, animated underline on hover)
- Trending Topics sidebar with animated number counters
- Weather widget + date/time display in header

**Main News Page Features**

- Masonry/grid card layout for latest articles
  - Card hover effect: slight lift + shadow bloom
  - Category colour-coded badges on each card
- Infinite scroll with skeleton loading animations
- Filter bar: date range, category, source, reading time
- Article count badges on category tabs
- Read-progress bar on individual article pages

**Article / Detail Page**

- Estimated reading time display
- Social share buttons with animated pop-up
- "Related Articles" carousel with slide-in animation
- Text resize controls for accessibility
- Dark-mode toggle (white ↔ deep navy)

**Content Collection Features**

- Automated RSS/API news crawler (runs every 15 minutes)
- Duplicate detection via content hash + similarity scoring
- AI-powered brief summary generation per article
- Source credibility scoring and blacklist management
- Auto-tagging of topics/categories with NLP

### 1.5 UI/UX Design Requirements

**Color System**

| Token | Hex Value | Usage |
| --- | --- | --- |
| `--color-bg-primary` | `#0A1628` | Site background / hero |
| `--color-bg-secondary` | `#1A2B4A` | Sidebar / card bg |
| `--color-accent` | `#2563EB` | CTA buttons / badges |
| `--color-accent-hover` | `#3B82F6` | Hover states |
| `--color-gold` | `#F59E0B` | Breaking news accent |
| `--color-surface` | `#FFFFFF` | Article cards |
| `--color-text-main` | `#1E293B` | Body text |
| `--color-text-muted` | `#475569` | Captions / meta |

**Animation Standards**

- Page transitions: 300ms ease-in-out fade + slide
- Card hovers: `transform: translateY(-4px)` + box-shadow bloom
- Hero banner: Ken Burns effect on background image (CSS)
- Ticker tape: continuous horizontal scroll animation
- Skeleton loaders: shimmer gradient animation while fetching
- Number counters: count-up animation on scroll-into-view

### 1.6 Content Strategy

- Only original content: no republished full articles; only summaries + source attribution
- Minimum 150-word AI-generated summary per article
- Source URL and original author always displayed
- Content categories: World, Tech, Business, Science, Health, Sports, Culture
- Editorial review queue before auto-publication (configurable)
- GDPR-compliant content storage and user data handling

---

## 2. Technical Architecture

### 2.1 Architecture Overview

> **Architecture Pattern: PERN Stack (PostgreSQL + Express + React + Node)**
> Three-tier architecture: React.js SPA on the frontend → Node.js/Express REST API in the middle tier → PostgreSQL with Prisma ORM as the data layer. An autonomous Content Ingestion Service runs as a separate Node.js process (cron-driven) that crawls, validates, and stores news content independently of the web server.

System components communicate through well-defined REST APIs. The frontend never touches the database directly. All data flows through the secured Express API layer. Redis caching sits between the API and database to reduce latency on high-traffic endpoints.

**High-Level Component Diagram**

| Layer | Component | Technology | Role |
| --- | --- | --- | --- |
| Presentation | SPA Frontend | React.js 18 | UI rendering & animations |
| Presentation | State Management | Redux Toolkit | Global state (auth, articles) |
| Application | REST API Server | Express.js + Node | Business logic & routing |
| Application | News Crawler | Node.js + Cron | Content ingestion service |
| Application | Cache Layer | Redis | Rate limiting & caching |
| Data | Primary DB | PostgreSQL 16 | Persistent storage |
| Data | ORM | Prisma 5 | Schema & query management |
| Data | File Storage | AWS S3 / Cloudinary | Images & media |

### 2.2 Frontend Architecture (React.js)

**Component Hierarchy**

- `App.jsx` → Router → Layout (Header + Footer)
  - Pages: Home | CategoryPage | ArticleDetail | SearchResults | Admin
  - Shared Components: Navbar | HeroBanner | ArticleCard | Ticker | Sidebar
  - UI Primitives: Button | Badge | Skeleton | Modal | Toast

**Key Frontend Libraries**

| Library | Version | Purpose |
| --- | --- | --- |
| React | 18.3 | Core UI framework |
| React Router DOM | 6.x | Client-side routing |
| Redux Toolkit | 2.x | State management |
| Framer Motion | 11.x | Page & component animations |
| GSAP | 3.12 | Advanced scroll animations |
| Axios | 1.7 | HTTP client with interceptors |
| TailwindCSS | 3.4 | Utility-first styling |
| Swiper.js | 11.x | Hero & article carousels |
| Lottie-react | 2.x | JSON-based micro-animations |
| React Query | 5.x | Server state & caching |

### 2.3 Backend Architecture (Node.js)

**Express API Structure**

- `/api/v1/articles`: CRUD + search + filtering
- `/api/v1/categories`: Category management
- `/api/v1/auth`: Login / register / refresh token
- `/api/v1/admin`: Editorial dashboard endpoints (protected)
- `/api/v1/crawler`: Manual trigger & status endpoints
- `/api/v1/health`: Service health check

**Middleware Stack**

| Middleware | Library | Function |
| --- | --- | --- |
| Authentication | jsonwebtoken | JWT verification on protected routes |
| Rate Limiting | express-rate-limit | 100 req/15 min per IP |
| CORS | cors | Whitelist frontend origin |
| Body Parsing | express.json() | JSON payload parsing |
| Logging | Morgan + Winston | Request & error logging |
| Validation | Zod | Schema validation on all inputs |
| Compression | compression | Gzip response compression |
| Helmet | helmet | HTTP security headers |

### 2.4 Database Architecture (PostgreSQL + Prisma)

**Core Prisma Schema: Key Models**

`User` │ `Article` │ `Category` │ `Tag` │ `ArticleTag` │ `Source` │ `CrawlerLog` │ `RefreshToken` │ `AuditLog`

**Article Table (Primary)**

| Field | Type | Constraint | Description |
| --- | --- | --- | --- |
| id | UUID | PK, Auto | Unique article identifier |
| title | String | NOT NULL | Article headline |
| slug | String | UNIQUE | URL-friendly identifier |
| summary | Text | NOT NULL | AI-generated brief (150+ words) |
| sourceUrl | String | NOT NULL | Original article URL |
| imageUrl | String | Nullable | Cover image URL |
| publishedAt | DateTime | NOT NULL | Original publish timestamp |
| categoryId | UUID | FK → Category | Category reference |
| sourceId | UUID | FK → Source | News source reference |
| isBreaking | Boolean | Default: false | Breaking news flag |
| viewCount | Integer | Default: 0 | Article view counter |
| contentHash | String | UNIQUE | Duplicate detection hash |
| status | Enum | DRAFT/PUBLISHED | Editorial status |
| createdAt | DateTime | Auto | Record creation time |

**Database Indexes for Performance**

- `idx_articles_published_at (DESC)`: latest articles query
- `idx_articles_category_status`: category page queries
- `idx_articles_content_hash`: duplicate detection
- `idx_articles_slug`: URL resolution
- Full-text search index on title + summary columns

### 2.5 API Design

**REST Endpoints: Articles**

| Method | Endpoint | Auth | Description |
| --- | --- | --- | --- |
| GET | `/api/v1/articles` | Public | Paginated article list with filters |
| GET | `/api/v1/articles/:slug` | Public | Single article by slug |
| GET | `/api/v1/articles/breaking` | Public | Breaking news (top 5) |
| GET | `/api/v1/articles/search` | Public | Full-text search |
| POST | `/api/v1/articles` | Admin | Create article manually |
| PATCH | `/api/v1/articles/:id` | Admin | Update article |
| DELETE | `/api/v1/articles/:id` | Admin | Soft-delete article |
| PATCH | `/api/v1/articles/:id/publish` | Admin | Toggle publish status |

---

## 3. System Workflow

### 3.1 Content Ingestion Workflow

> **Automated News Collection Pipeline**
> Every 15 minutes, the Content Ingestion Service wakes up, fetches news from configured RSS feeds and public news APIs, validates originality, generates AI summaries, and stages articles for publication.

**Step-by-Step Ingestion Flow**

| Step | Process | Component | Action on Failure |
| --- | --- | --- | --- |
| 1 | Scheduler triggers crawl job | Node-Cron | Log & retry next cycle |
| 2 | Fetch RSS feeds & API sources | Axios + rss-parser | Skip failed sources |
| 3 | Extract title, URL, date, author | Cheerio scraper | Discard malformed entries |
| 4 | Generate content hash (SHA-256) | crypto module | Skip if hash exists |
| 5 | Check duplicate in DB by hash+URL | Prisma query | Skip duplicates |
| 6 | Validate source credibility score | Source model | Reject score < 60 |
| 7 | Fetch full article text (headless) | Puppeteer/fetch | Use RSS excerpt |
| 8 | Generate AI summary (150+ words) | Claude API call | Use scraped excerpt |
| 9 | Auto-tag categories via NLP | compromise.js | Use default category |
| 10 | Store article with DRAFT status | Prisma create | Retry 3x, then log |
| 11 | Notify admin dashboard (WebSocket) | Socket.io | Ignore if WS closed |
| 12 | Auto-publish if credibility > 85 | Business logic | Leave as DRAFT |

### 3.2 User Interaction Workflow

**Homepage Load Flow**

- Browser → CDN → React SPA served (HTML shell + JS bundle)
- React hydrates → Redux initialises → React Query fires parallel requests:
  - `GET /api/v1/articles/breaking` (hero banner data)
  - `GET /api/v1/articles?limit=12` (main grid)
  - `GET /api/v1/articles?sort=trending&limit=5` (sidebar)
- Skeleton loaders display while API responds
- Framer Motion animates article cards in with staggered fade-up
- WebSocket connection opened for live breaking-news alerts

**Article View Flow**

| Action | Frontend | Backend | Database |
| --- | --- | --- | --- |
| User clicks article card | Navigate to `/article/:slug` | n/a | n/a |
| React Router matches route | ArticleDetail page mounts | n/a | n/a |
| Data fetch triggered | `useQuery(slug)` | `GET /articles/:slug` | SELECT by slug |
| View count increment | n/a | `PATCH /articles/:id/view` | UPDATE viewCount++ |
| Page renders | Framer Motion fade-in | n/a | n/a |
| Related articles fetch | `useQuery(categoryId)` | `GET /articles?category=X&limit=4` | SELECT category |
| Social share clicked | Open share modal (animated) | n/a | n/a |

### 3.3 Admin Workflow

**Editorial Dashboard Flow**

- Admin logs in → JWT issued (15 min access + 7 day refresh)
- Dashboard loads: pending DRAFT articles queue
- Admin reviews article: approve / reject / edit summary
- On approve: status → PUBLISHED, publishedAt set, cache invalidated
- On reject: article soft-deleted, source score decremented

**Crawler Management**

- Admin can add/remove/pause news sources via Source Manager
- Credibility score manually adjustable per source
- Crawler logs viewable: success count, duplicates skipped, errors
- Manual "Run Crawler Now" button for on-demand collection

**Content Moderation Rules Engine**

| Rule | Condition | Automatic Action |
| --- | --- | --- |
| Duplicate Guard | contentHash already in DB | Discard silently |
| Low Credibility | Source score < 60 | Reject + log |
| Auto-Publish | Source score > 85 + no flagged words | Publish immediately |
| Manual Review | Score 60–85 | Place in DRAFT queue |
| Blacklisted Keywords | Title/summary contains banned words | Hold for editor review |
| Stale Content | publishedAt > 48 hours ago | Skip ingestion |

---

## 4. Security Architecture

> **Security Philosophy**
> Defence-in-depth: every layer of the stack implements independent security controls. The frontend never trusts its own state for authorization; every privileged action is re-verified server-side. Secrets never leave the server; public content is served read-only without authentication.

### 4.1 Authentication & Authorization

**JWT Token Strategy**

| Token Type | Expiry | Storage | Rotation Policy |
| --- | --- | --- | --- |
| Access Token | 15 minutes | Memory (Redux state) | Reissued via refresh token |
| Refresh Token | 7 days | HTTP-only Secure cookie | Rotated on each use |
| Crawler API Key | Static | Environment variable only | Manual rotation |
| Admin Invite Code | 24 hours | DB: one-time use UUID | Invalidated on use |

**Role-Based Access Control (RBAC)**

| Role | Permissions |
| --- | --- |
| Public Reader | Read published articles, search, share |
| Registered | Reader permissions + bookmarks, reading history |
| Editor | Registered + approve/reject DRAFT articles, edit summaries |
| Admin | Editor + manage sources, users, crawler settings, blacklists |
| Super Admin | Admin + system settings, delete users, database operations |

### 4.2 Data Security

**Encryption Standards**

- Passwords: bcrypt with salt rounds = 12 (never stored in plain text)
- Refresh tokens: hashed (SHA-256) before DB storage
- Database: TLS encryption in transit (SSL mode = require)
- Environment secrets: stored in `.env` (gitignored) + production secrets manager
- API keys: never exposed in frontend bundle (server-side only)

**Data Validation & Sanitisation**

- All API inputs validated with Zod schema before processing
- HTML content stripped via DOMPurify before rendering (XSS prevention)
- URL inputs validated against whitelist of permitted source domains
- File uploads: MIME-type checked + file size limit (5 MB max)

### 4.3 API Security

| Threat | Mitigation | Library/Method |
| --- | --- | --- |
| Brute Force Login | Rate limit: 5 attempts/15 min | express-rate-limit |
| SQL Injection | Parameterised queries via Prisma | Prisma ORM (no raw SQL) |
| XSS Attacks | Output encoding + CSP header | Helmet + DOMPurify |
| CSRF Attacks | SameSite=Strict cookie flag | Cookie configuration |
| DDoS / Flood | IP rate limit + CDN WAF | Cloudflare / express-slow-down |
| Credential Stuffing | Account lockout + CAPTCHA | Custom + hCaptcha |
| IDOR (Object Access) | Ownership check on every query | Prisma `where: { userId }` |
| Dependency Vulns | Weekly npm audit + Dependabot | GitHub Dependabot |

### 4.4 Content Security

**Content Security Policy (CSP) Headers**

- `default-src 'self'`
- `script-src 'self' 'nonce-{random}'` (no unsafe-inline)
- `img-src 'self' data: https://images.yourdomain.com https://res.cloudinary.com`
- `connect-src 'self' https://api.yourdomain.com`
- `frame-ancestors 'none'` (clickjacking prevention)

**Infrastructure Security**

| Component | Security Measure |
| --- | --- |
| Node.js Server | Run as non-root user; PM2 process manager |
| PostgreSQL | Private subnet; no public IP; pg_hba.conf restricted |
| Redis | Password protected; bind to localhost only |
| HTTPS | TLS 1.3 only; HSTS header; auto-cert via Let's Encrypt |
| Secrets | Vault / AWS Secrets Manager in production |
| Logs | No PII in logs; log rotation; 30-day retention |
| Docker | Non-root containers; read-only filesystem where possible |

---

## 5. Technology Stack

### 5.1 Full Stack Overview

| Category | Technology | Version | Role |
| --- | --- | --- | --- |
| Frontend Framework | React.js | 18.3.x | Core UI component library |
| Build Tool | Vite | 5.x | Dev server & production bundler |
| Routing | React Router DOM | 6.x | Client-side SPA routing |
| State (Global) | Redux Toolkit | 2.x | App-wide state management |
| State (Server) | TanStack Query (React Query) | 5.x | API data caching & sync |
| HTTP Client | Axios | 1.7.x | API communication |
| Styling | TailwindCSS | 3.4.x | Utility-first CSS framework |
| UI Components | shadcn/ui | latest | Accessible pre-built components |
| Runtime | Node.js | 20 LTS | Server JavaScript runtime |
| Web Framework | Express.js | 4.x | REST API framework |
| ORM | Prisma | 5.x | Database schema & queries |
| Validation | Zod | 3.x | Runtime type validation |
| Authentication | jsonwebtoken | 9.x | JWT generation & verification |
| Password Hashing | bcryptjs | 2.x | Secure password storage |
| Scheduling | node-cron | 3.x | Crawler job scheduling |
| Web Scraping | Cheerio + Puppeteer | latest | Content extraction |
| RSS Parsing | rss-parser | 3.x | Feed ingestion |
| Real-time | Socket.io | 4.x | Live breaking-news push |
| Primary Database | PostgreSQL | 16.x | Relational data store |
| Cache | Redis | 7.x | Session cache & rate limiting |
| File Storage | Cloudinary | latest | Image upload & optimisation |
| Process Manager | PM2 | 5.x | Node.js process management |
| Containerisation | Docker + Compose | latest | Dev & prod environment parity |

### 5.2 Animation & UI Libraries

| Library | Version | Animations Handled |
| --- | --- | --- |
| Framer Motion | 11.x | Page transitions, card hover, modal enter/exit, stagger effects |
| GSAP | 3.12 | Hero banner Ken Burns, scroll-triggered counters, ticker |
| Lottie-react | 2.x | Micro-animations: loading spinners, empty states, success |
| Swiper.js | 11.x | Hero image carousel, related-articles slider, category scroll |
| React Spring | 9.x | Physics-based number counters, like/bookmark pop |
| AOS (Animate on Scroll) | 3.x | Fade-in cards as user scrolls down the page |
| CSS Animations | native | Ticker tape, skeleton shimmer, gradient pulsing on banner |

> **Animation Performance Guidelines**
> All animations must use CSS `transform` and `opacity` exclusively; never animate width, height, top, left, or background-color directly. Use `will-change` sparingly. Target 60 fps (16ms frame budget). Honour the `prefers-reduced-motion` media query: all animations gracefully disabled for users who set this accessibility preference.

### 5.3 DevOps & Infrastructure

| Category | Tool | Purpose |
| --- | --- | --- |
| Version Control | Git + GitHub | Source code management & PR reviews |
| CI/CD Pipeline | GitHub Actions | Auto-test, build, and deploy on push |
| Hosting (Frontend) | Vercel / Netlify | Global CDN + instant edge deployment |
| Hosting (Backend) | AWS EC2 / Railway | Node.js API server hosting |
| Database Hosting | Supabase / AWS RDS | Managed PostgreSQL with backups |
| Monitoring | Sentry + Datadog | Error tracking & performance APM |
| Logging | Winston + Logtail | Structured application logs |
| Testing | Vitest + Supertest | Unit + integration test suite |
| E2E Testing | Playwright | Browser-level end-to-end tests |
| Code Quality | ESLint + Prettier | Consistent code style enforcement |
| API Docs | Swagger / OpenAPI 3 | Auto-generated API documentation |

### 5.4 Version Matrix & Compatibility

| Package | Pinned Version | Node Requirement | Notes |
| --- | --- | --- | --- |
| react | 18.3.1 | ≥ 18.0 | Concurrent Mode enabled |
| react-dom | 18.3.1 | ≥ 18.0 | Pairs with react version |
| vite | 5.4.x | ≥ 18.0 | ESM-first build |
| tailwindcss | 3.4.x | ≥ 16.0 | JIT mode default |
| framer-motion | 11.x | ≥ 18.0 | React 18 Concurrent safe |
| express | 4.19.x | ≥ 18.0 | v5 not yet stable |
| @prisma/client | 5.18.x | ≥ 18.0 | PostgreSQL provider |
| prisma (CLI) | 5.18.x | ≥ 18.0 | Schema migrations |
| jsonwebtoken | 9.0.2 | ≥ 14.0 | ES2015 compatible |
| socket.io | 4.7.x | ≥ 16.0 | WebSocket + long-poll fallback |
| node-cron | 3.0.x | ≥ 14.0 | POSIX cron syntax |
| zod | 3.23.x | ≥ 16.0 | TypeScript-first validation |

---

*End of document. The Digital Chronicle PRD v1.0.*

**Next Steps:** Begin Sprint 1 (Project Scaffolding, DB Schema & CI/CD Pipeline Setup)
