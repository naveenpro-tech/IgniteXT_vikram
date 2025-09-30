# IgniteXT - Technology Stack

## Frontend Technologies

### Core Framework
- **Next.js 14+** (App Router)
  - React 18+ with Server Components
  - TypeScript for type safety
  - Built-in API routes
  - Image optimization
  - SEO-friendly SSR/SSG

### UI & Styling
- **Tailwind CSS** - Utility-first CSS framework
- **shadcn/ui** - High-quality React components
- **Radix UI** - Accessible component primitives
- **Lucide Icons** - Beautiful icon library
- **Framer Motion** - Animation library

### State Management
- **Zustand** - Lightweight state management
- **React Query (TanStack Query)** - Server state management
- **React Context** - For theme and auth state

### Forms & Validation
- **React Hook Form** - Performant form library
- **Zod** - TypeScript-first schema validation

### Rich Text & Code
- **Tiptap** - Headless rich text editor
- **Monaco Editor** - VS Code's editor for code problems
- **React Markdown** - Markdown rendering

### Charts & Visualization
- **Recharts** - Composable charting library
- **React Flow** - Interactive node-based graphs (for roadmaps)

### Date & Time
- **date-fns** - Modern date utility library

## Backend Technologies

### Backend-as-a-Service
- **Supabase**
  - PostgreSQL database
  - Authentication (Email, OAuth, Phone)
  - Real-time subscriptions
  - Storage for files
  - Edge Functions
  - Row Level Security (RLS)

### API Layer
- **Next.js API Routes** - Serverless functions
- **tRPC** (Optional) - End-to-end type-safe APIs

## Database

### Primary Database
- **PostgreSQL** (via Supabase)
  - Relational data modeling
  - JSONB for flexible data
  - Full-text search
  - Triggers and functions

### Caching (Future)
- **Redis** - For session storage and caching

## Authentication & Authorization

- **Supabase Auth**
  - Email/Password authentication
  - Google OAuth
  - Phone (OTP) authentication
  - JWT tokens
  - Role-based access control (RBAC)

## File Storage

- **Supabase Storage**
  - User avatars
  - Workshop materials
  - Resource files (PDFs, videos)
  - Image uploads

## AI & Machine Learning

### Phase 1
- **Rule-based chatbot** - Predefined responses

### Phase 2-3
- **OpenAI GPT-4 API** - Intelligent chatbot
- **OpenAI Embeddings** - Semantic search
- **Langchain** (Optional) - LLM orchestration

## Email Services

- **Resend** (Primary choice)
  - Modern email API
  - React Email templates
  - Good deliverability

- **SendGrid** (Alternative)
  - Established provider
  - Template management

## Payment Processing (Future)

- **Razorpay** (India-focused)
- **Stripe** (Global)

## Deployment & Hosting

### Frontend Hosting
- **Vercel**
  - Optimized for Next.js
  - Edge Network CDN
  - Automatic HTTPS
  - Preview deployments
  - Analytics built-in

### Database Hosting
- **Supabase Cloud**
  - Managed PostgreSQL
  - Automatic backups
  - Global distribution

### Domain & DNS
- **Cloudflare** (Optional) - DNS management, DDoS protection

## Development Tools

### Package Manager
- **pnpm** - Fast, disk space efficient

### Code Quality
- **ESLint** - JavaScript/TypeScript linting
- **Prettier** - Code formatting
- **Husky** - Git hooks
- **lint-staged** - Run linters on staged files

### Testing
- **Jest** - Unit testing
- **React Testing Library** - Component testing
- **Playwright** - End-to-end testing
- **MSW (Mock Service Worker)** - API mocking

### Type Safety
- **TypeScript** - Static type checking
- **Zod** - Runtime type validation

## Monitoring & Analytics

### Error Tracking
- **Sentry** - Error monitoring and tracking

### Analytics
- **Vercel Analytics** - Web analytics
- **Google Analytics 4** - User behavior tracking
- **PostHog** (Optional) - Product analytics

### Performance Monitoring
- **Vercel Speed Insights** - Core Web Vitals
- **Lighthouse CI** - Automated performance testing

### Uptime Monitoring
- **UptimeRobot** - Uptime monitoring
- **Pingdom** (Alternative)

## CI/CD

- **GitHub Actions**
  - Automated testing
  - Linting and type checking
  - Build verification
  - Automated deployments

## Version Control

- **Git** - Version control
- **GitHub** - Code hosting and collaboration

## Communication & Collaboration

### Project Management
- **Linear** - Issue tracking and project management
- **GitHub Projects** (Alternative)

### Design
- **Figma** - UI/UX design and prototyping

### Documentation
- **Markdown** - Documentation format
- **Storybook** (Optional) - Component documentation

## Progressive Web App (PWA)

- **next-pwa** - PWA plugin for Next.js
- **Workbox** - Service worker library
- **Web Push API** - Push notifications

## Security

- **HTTPS** - Automatic via Vercel
- **Content Security Policy (CSP)** - XSS protection
- **Rate Limiting** - API protection
- **Helmet.js** - Security headers
- **CORS** - Cross-origin resource sharing

## Development Environment

### Required Software
- **Node.js 18+** - JavaScript runtime
- **pnpm** - Package manager
- **Git** - Version control
- **VS Code** - Recommended IDE

### VS Code Extensions
- ESLint
- Prettier
- Tailwind CSS IntelliSense
- TypeScript and JavaScript Language Features
- GitLens
- Supabase

## Environment Variables

```bash
# Supabase
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_ANON_KEY=
SUPABASE_SERVICE_ROLE_KEY=

# OpenAI (Phase 3)
OPENAI_API_KEY=

# Email
RESEND_API_KEY=

# Analytics
NEXT_PUBLIC_GA_MEASUREMENT_ID=
SENTRY_DSN=

# App
NEXT_PUBLIC_APP_URL=
```

## Cost Breakdown (Monthly)

### Free Tier (MVP)
- Vercel Hobby: $0
- Supabase Free: $0
- GitHub: $0
- **Total: $0/month**

### Production (1000-10000 users)
- Vercel Pro: $20
- Supabase Pro: $25
- Resend: $10
- Sentry Team: $26
- Domain: $1.25 (annual/12)
- **Total: ~$82/month**

### Scale (10000+ users)
- Vercel Enterprise: Custom
- Supabase Team: $599
- Resend: $50
- Sentry Business: $80
- Redis: $30
- **Total: ~$800+/month**

## Technology Decision Rationale

### Why This Stack?

1. **Developer Productivity**: Modern tools with great DX
2. **Type Safety**: TypeScript throughout the stack
3. **Performance**: Optimized for speed and SEO
4. **Scalability**: Can handle growth from 0 to 100k+ users
5. **Cost-Effective**: Free tier for MVP, reasonable scaling costs
6. **Community**: Large communities for support
7. **Maintenance**: Easy to maintain and update
8. **Hiring**: Popular technologies, easy to find developers

### Alternatives Considered

| Technology | Alternative | Why Not Chosen |
|------------|-------------|----------------|
| Next.js | Remix, Astro | Next.js has better ecosystem and Vercel integration |
| Supabase | Firebase, AWS Amplify | Supabase offers PostgreSQL and better pricing |
| Tailwind | Styled Components, CSS Modules | Tailwind is faster for prototyping |
| Vercel | Netlify, AWS | Vercel is optimized for Next.js |
| PostgreSQL | MongoDB, MySQL | PostgreSQL is more powerful for complex queries |

## Future Technology Additions

### When Scaling
- **Redis** - Caching and session storage
- **BullMQ** - Job queue for background tasks
- **Elasticsearch** - Advanced search capabilities
- **WebSockets** - Real-time features beyond Supabase
- **CDN** - Additional CDN for media files
- **Microservices** - Split heavy features into separate services

### Mobile App (Phase 4)
- **React Native** or **Flutter** - Native mobile apps
- **Expo** - React Native framework
- **Firebase Cloud Messaging** - Push notifications

---

**Last Updated**: 2025-09-30  
**Version**: 1.0

