# IgniteXT - System Architecture

## 🏗️ High-Level Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                         CLIENT LAYER                             │
├─────────────────────────────────────────────────────────────────┤
│  Web Browser (Desktop/Mobile)  │  PWA (Installable)             │
│  - React Components             │  - Service Worker              │
│  - Next.js App Router           │  - Offline Support             │
│  - Tailwind CSS                 │  - Push Notifications          │
└─────────────────────────────────────────────────────────────────┘
                              ↓ HTTPS
┌─────────────────────────────────────────────────────────────────┐
│                      VERCEL EDGE NETWORK                         │
├─────────────────────────────────────────────────────────────────┤
│  - CDN (Global)                                                  │
│  - SSL/TLS Termination                                           │
│  - DDoS Protection                                               │
│  - Edge Functions                                                │
└─────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│                    APPLICATION LAYER (Next.js)                   │
├─────────────────────────────────────────────────────────────────┤
│  Server Components          │  API Routes                        │
│  - SSR/SSG Pages            │  - REST Endpoints                  │
│  - Data Fetching            │  - Webhooks                        │
│  - Authentication           │  - Cron Jobs                       │
│                             │                                    │
│  Client Components          │  Middleware                        │
│  - Interactive UI           │  - Auth Guard                      │
│  - State Management         │  - Rate Limiting                   │
│  - Real-time Updates        │  - Logging                         │
└─────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│                    BACKEND SERVICES (Supabase)                   │
├─────────────────────────────────────────────────────────────────┤
│  PostgreSQL Database        │  Authentication                    │
│  - User Data                │  - Email/Password                  │
│  - Roadmaps                 │  - OAuth (Google)                  │
│  - Workshops/Contests       │  - Phone (OTP)                     │
│  - Community Posts          │  - JWT Tokens                      │
│  - Progress Tracking        │  - RLS Policies                    │
│                             │                                    │
│  Storage                    │  Real-time                         │
│  - User Avatars             │  - WebSocket Subscriptions         │
│  - Workshop Materials       │  - Live Notifications              │
│  - Resource Files           │  - Chat Messages                   │
│                             │                                    │
│  Edge Functions             │  Triggers                          │
│  - Background Jobs          │  - Auto-moderation                 │
│  - Email Sending            │  - Badge Awards                    │
│  - Data Processing          │  - Point Calculations              │
└─────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│                      EXTERNAL SERVICES                           │
├─────────────────────────────────────────────────────────────────┤
│  OpenAI API                 │  Email Service (Resend)            │
│  - GPT-4 Chatbot            │  - Transactional Emails            │
│  - Recommendations          │  - Notifications                   │
│                             │                                    │
│  Analytics                  │  Monitoring                        │
│  - Vercel Analytics         │  - Sentry (Errors)                 │
│  - Google Analytics         │  - Uptime Monitoring               │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🔄 Data Flow Architecture

### User Authentication Flow
```
User → Login Page → Supabase Auth → JWT Token → Protected Routes
                         ↓
                    Profile Created
                         ↓
                    Session Stored
                         ↓
                    Redirect to Dashboard
```

### Roadmap Progress Tracking Flow
```
User Views Roadmap → Fetch Roadmap Data → Display Nodes
                                              ↓
User Marks Node Complete → Update Progress → Award Points
                                              ↓
                                         Check Badges
                                              ↓
                                         Update Leaderboard
                                              ↓
                                         Send Notification
```

### Workshop Registration Flow
```
User Browses Workshops → Select Workshop → Check Availability
                                              ↓
                                         Register
                                              ↓
                                    Send Confirmation Email
                                              ↓
                                    Add to Calendar
                                              ↓
                                    Send Reminders (24h, 1h)
```

### Mentorship Booking Flow
```
User Browses Mentors → View Availability → Select Time Slot
                                              ↓
                                         Book Session
                                              ↓
                                    Generate Meeting Link
                                              ↓
                                    Send Confirmation
                                              ↓
                                    Add to Both Calendars
```

---

## 🗄️ Database Architecture

### Entity Relationship Overview

```
profiles (users)
    ↓ 1:N
user_roadmap_progress
    ↓ N:1
roadmap_nodes
    ↓ N:1
roadmaps

profiles
    ↓ 1:N
workshop_registrations
    ↓ N:1
workshops

profiles
    ↓ 1:N
contest_participants
    ↓ N:1
contests

profiles (mentors)
    ↓ 1:N
mentorship_sessions
    ↓ N:1
profiles (mentees)

profiles
    ↓ 1:N
community_posts
    ↓ 1:N
post_comments

profiles
    ↓ N:N (via user_badges)
badges
```

### Row Level Security (RLS) Policies

```sql
-- Users can only read their own profile
CREATE POLICY "Users can view own profile"
ON profiles FOR SELECT
USING (auth.uid() = id);

-- Users can update their own profile
CREATE POLICY "Users can update own profile"
ON profiles FOR UPDATE
USING (auth.uid() = id);

-- Anyone can view published roadmaps
CREATE POLICY "Anyone can view published roadmaps"
ON roadmaps FOR SELECT
USING (is_published = true);

-- Users can only view their own progress
CREATE POLICY "Users can view own progress"
ON user_roadmap_progress FOR SELECT
USING (auth.uid() = user_id);

-- Users can only register themselves for workshops
CREATE POLICY "Users can register for workshops"
ON workshop_registrations FOR INSERT
WITH CHECK (auth.uid() = user_id);
```

---

## 🔐 Security Architecture

### Authentication & Authorization

```
┌─────────────────────────────────────────────────────────────┐
│                    Security Layers                           │
├─────────────────────────────────────────────────────────────┤
│  1. HTTPS/TLS                                                │
│     - All traffic encrypted                                  │
│     - Automatic via Vercel                                   │
├─────────────────────────────────────────────────────────────┤
│  2. Authentication                                           │
│     - Supabase Auth (JWT)                                    │
│     - Secure password hashing                                │
│     - OAuth 2.0 for Google                                   │
├─────────────────────────────────────────────────────────────┤
│  3. Authorization                                            │
│     - Role-based access control (RBAC)                       │
│     - Row Level Security (RLS)                               │
│     - API route protection                                   │
├─────────────────────────────────────────────────────────────┤
│  4. Input Validation                                         │
│     - Zod schema validation                                  │
│     - SQL injection prevention                               │
│     - XSS protection                                         │
├─────────────────────────────────────────────────────────────┤
│  5. Rate Limiting                                            │
│     - API endpoint throttling                                │
│     - Login attempt limiting                                 │
│     - DDoS protection                                        │
├─────────────────────────────────────────────────────────────┤
│  6. Content Security                                         │
│     - Content Security Policy (CSP)                          │
│     - CORS configuration                                     │
│     - Secure headers                                         │
└─────────────────────────────────────────────────────────────┘
```

---

## 📊 State Management Architecture

### Client-Side State

```
┌─────────────────────────────────────────────────────────────┐
│                    State Management                          │
├─────────────────────────────────────────────────────────────┤
│  Zustand Stores                                              │
│  - authStore (user session)                                  │
│  - uiStore (theme, sidebar, modals)                          │
│  - notificationStore (toast notifications)                   │
├─────────────────────────────────────────────────────────────┤
│  React Query (TanStack Query)                                │
│  - Server state caching                                      │
│  - Automatic refetching                                      │
│  - Optimistic updates                                        │
│  - Infinite queries (pagination)                             │
├─────────────────────────────────────────────────────────────┤
│  React Context                                               │
│  - Theme context (dark/light mode)                           │
│  - Auth context (user data)                                  │
└─────────────────────────────────────────────────────────────┘
```

---

## 🚀 Deployment Architecture

### CI/CD Pipeline

```
Developer Push to GitHub
        ↓
GitHub Actions Triggered
        ↓
    Run Tests
        ↓
    Run Linting
        ↓
    Build Application
        ↓
    [Branch: develop] → Deploy to Staging (Vercel Preview)
    [Branch: main] → Deploy to Production (Vercel)
        ↓
    Run Database Migrations (Supabase)
        ↓
    Deployment Complete
        ↓
    Send Notification (Slack/Discord)
```

### Environment Strategy

```
┌──────────────────────────────────────────────────────────┐
│  Development (Local)                                      │
│  - localhost:3000                                         │
│  - Local Supabase (optional)                              │
│  - .env.local                                             │
├──────────────────────────────────────────────────────────┤
│  Staging (Vercel Preview)                                 │
│  - staging.ignitext.com                                   │
│  - Supabase staging project                               │
│  - .env.staging                                           │
├──────────────────────────────────────────────────────────┤
│  Production (Vercel)                                      │
│  - ignitext.com                                           │
│  - Supabase production project                            │
│  - .env.production                                        │
└──────────────────────────────────────────────────────────┘
```

---

## 📈 Scalability Architecture

### Scaling Strategy

```
Phase 1: 0-1,000 users
├─ Vercel Hobby Plan
├─ Supabase Free Plan
└─ Single region deployment

Phase 2: 1,000-10,000 users
├─ Vercel Pro Plan
├─ Supabase Pro Plan
├─ CDN for static assets
├─ Database query optimization
└─ Redis caching (optional)

Phase 3: 10,000-100,000 users
├─ Vercel Enterprise
├─ Supabase Team/Enterprise
├─ Database read replicas
├─ Queue system (BullMQ)
├─ Microservices for heavy features
└─ Load balancing

Phase 4: 100,000+ users
├─ Multi-region deployment
├─ Database sharding
├─ Dedicated infrastructure
├─ Advanced caching strategies
└─ Kubernetes orchestration
```

---

## 🔄 Real-time Architecture

### Supabase Real-time Subscriptions

```typescript
// Subscribe to new notifications
const channel = supabase
  .channel('user-notifications')
  .on(
    'postgres_changes',
    {
      event: 'INSERT',
      schema: 'public',
      table: 'notifications',
      filter: `user_id=eq.${userId}`,
    },
    (payload) => {
      // Handle new notification
      showToast(payload.new)
    }
  )
  .subscribe()

// Subscribe to community post updates
const postChannel = supabase
  .channel('community-posts')
  .on(
    'postgres_changes',
    {
      event: '*',
      schema: 'public',
      table: 'community_posts',
    },
    (payload) => {
      // Update UI in real-time
      updatePostsList(payload)
    }
  )
  .subscribe()
```

---

## 🎨 Component Architecture

### Component Hierarchy

```
App Layout
├── Header
│   ├── Logo
│   ├── Navigation
│   ├── Search
│   └── UserMenu
├── Sidebar (Dashboard)
│   ├── Navigation Links
│   ├── Progress Widget
│   └── Quick Actions
├── Main Content
│   ├── Page Header
│   ├── Content Area
│   │   ├── Feature Components
│   │   │   ├── Roadmap Viewer
│   │   │   ├── Workshop Card
│   │   │   ├── Contest Card
│   │   │   ├── Mentor Card
│   │   │   └── Community Post
│   │   └── Shared Components
│   │       ├── Loading States
│   │       ├── Error Boundaries
│   │       └── Empty States
│   └── Sidebar (Content)
└── Footer
    ├── Links
    ├── Social Media
    └── Copyright
```

---

## 🧪 Testing Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    Testing Pyramid                           │
├─────────────────────────────────────────────────────────────┤
│  E2E Tests (Playwright)                                      │
│  - User flows                                                │
│  - Critical paths                                            │
│  - Cross-browser                                             │
├─────────────────────────────────────────────────────────────┤
│  Integration Tests                                           │
│  - API endpoints                                             │
│  - Database operations                                       │
│  - Auth flows                                                │
├─────────────────────────────────────────────────────────────┤
│  Unit Tests (Jest)                                           │
│  - Utility functions                                         │
│  - Custom hooks                                              │
│  - Components                                                │
└─────────────────────────────────────────────────────────────┘
```

---

## 📊 Monitoring Architecture

```
Application
    ↓
Error Tracking (Sentry)
    ↓
Performance Monitoring (Vercel Speed Insights)
    ↓
Analytics (Google Analytics + Vercel Analytics)
    ↓
Uptime Monitoring (UptimeRobot)
    ↓
Alerts (Email/Slack)
```

---

**Document Version**: 1.0  
**Last Updated**: 2025-09-30  
**Status**: Living Document

