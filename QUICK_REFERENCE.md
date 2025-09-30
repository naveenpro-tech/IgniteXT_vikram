# IgniteXT - Quick Reference Guide

## 📋 Project Overview

**Mission**: Student-driven community platform for coding education, career guidance, and mentorship

**Timeline**: 20-22 weeks (5-6 months)

**Tech Stack**: Next.js 14 + TypeScript + Supabase + Tailwind CSS

---

## 🎯 Core Features Summary

### Phase 1: MVP (4-6 weeks)
- ✅ User authentication (Email, Google OAuth)
- ✅ Interactive roadmaps (DSA, Web Dev)
- ✅ Workshop registration system
- ✅ Contest participation tracking
- ✅ Homepage with announcements
- ✅ Basic chatbot

### Phase 2: Engagement (6-8 weeks)
- ✅ Progress tracking & analytics
- ✅ Badges & achievements
- ✅ Leaderboards
- ✅ 1:1 Mentorship booking
- ✅ Resource library
- ✅ Community forum

### Phase 3: Expansion (6-8 weeks)
- ✅ Career options explorer
- ✅ Mental wellness support
- ✅ Practice problems bank
- ✅ Admin dashboard
- ✅ Sponsor management
- ✅ AI-powered chatbot
- ✅ PWA conversion

---

## 🗂️ Database Tables Quick Reference

### Core Tables
- `profiles` - User profiles with gamification data
- `roadmaps` - Learning roadmaps
- `roadmap_nodes` - Individual topics in roadmaps
- `user_roadmap_progress` - User progress tracking

### Events
- `workshops` - Workshop events
- `workshop_registrations` - User registrations
- `contests` - Coding contests
- `contest_participants` - Contest participation

### Mentorship
- `mentorship_sessions` - Booking sessions
- `mentor_availability` - Mentor time slots

### Community
- `community_posts` - Forum posts
- `post_comments` - Comments on posts
- `resources` - Learning resources

### Gamification
- `badges` - Available badges
- `user_badges` - Earned badges
- `practice_problems` - Coding problems
- `user_problem_submissions` - Problem solutions

### Admin
- `announcements` - Homepage announcements
- `sponsors` - Sponsor information
- `notifications` - User notifications

---

## 🔑 Key API Endpoints

### Authentication
- `POST /api/auth/signup` - Register
- `POST /api/auth/login` - Login
- `GET /api/auth/me` - Current user

### Roadmaps
- `GET /api/roadmaps` - List roadmaps
- `GET /api/roadmaps/:slug` - Roadmap details
- `POST /api/roadmaps/:id/progress` - Update progress

### Workshops
- `GET /api/workshops` - List workshops
- `POST /api/workshops/:id/register` - Register

### Mentorship
- `GET /api/mentorship/mentors` - List mentors
- `POST /api/mentorship/sessions` - Book session

### Community
- `GET /api/community/posts` - List posts
- `POST /api/community/posts` - Create post

### Admin
- `GET /api/admin/dashboard` - Admin stats
- `POST /api/admin/workshops` - Create workshop

---

## 📁 Project Structure

```
src/
├── app/                    # Next.js App Router
│   ├── (auth)/            # Auth pages
│   ├── (dashboard)/       # User dashboard
│   ├── (admin)/           # Admin panel
│   ├── (public)/          # Public pages
│   └── api/               # API routes
├── components/
│   ├── ui/                # shadcn/ui components
│   ├── layout/            # Layout components
│   ├── features/          # Feature components
│   └── shared/            # Shared components
├── lib/
│   ├── supabase/          # Supabase clients
│   └── utils.ts           # Utility functions
├── hooks/                 # Custom React hooks
├── store/                 # Zustand stores
└── types/                 # TypeScript types
```

---

## 🚀 Common Commands

### Development
```bash
pnpm dev                   # Start dev server
pnpm build                 # Build for production
pnpm start                 # Start production server
pnpm lint                  # Run linter
```

### Supabase
```bash
pnpm supabase start        # Start local Supabase
pnpm supabase db push      # Push migrations
pnpm supabase gen types    # Generate TypeScript types
```

### Testing
```bash
pnpm test                  # Run unit tests
pnpm test:e2e              # Run E2E tests
```

---

## 🎨 UI Component Examples

### Button
```tsx
import { Button } from '@/components/ui/button'

<Button variant="default">Click me</Button>
<Button variant="outline">Outline</Button>
<Button variant="ghost">Ghost</Button>
```

### Form
```tsx
import { useForm } from 'react-hook-form'
import { zodResolver } from '@hookform/resolvers/zod'
import * as z from 'zod'

const schema = z.object({
  email: z.string().email(),
  password: z.string().min(8),
})

const form = useForm({
  resolver: zodResolver(schema),
})
```

### Dialog
```tsx
import { Dialog, DialogContent, DialogHeader, DialogTitle } from '@/components/ui/dialog'

<Dialog open={open} onOpenChange={setOpen}>
  <DialogContent>
    <DialogHeader>
      <DialogTitle>Title</DialogTitle>
    </DialogHeader>
    {/* Content */}
  </DialogContent>
</Dialog>
```

---

## 🔐 Authentication Patterns

### Client Component
```tsx
'use client'
import { createClient } from '@/lib/supabase/client'

const supabase = createClient()
const { data: { user } } = await supabase.auth.getUser()
```

### Server Component
```tsx
import { createClient } from '@/lib/supabase/server'

const supabase = createClient()
const { data: { user } } = await supabase.auth.getUser()
```

### Protected Route
```tsx
import { redirect } from 'next/navigation'
import { createClient } from '@/lib/supabase/server'

export default async function ProtectedPage() {
  const supabase = createClient()
  const { data: { user } } = await supabase.auth.getUser()
  
  if (!user) {
    redirect('/login')
  }
  
  return <div>Protected content</div>
}
```

---

## 📊 Database Query Examples

### Fetch with RLS
```typescript
const { data, error } = await supabase
  .from('roadmaps')
  .select('*')
  .eq('is_published', true)
```

### Insert
```typescript
const { data, error } = await supabase
  .from('profiles')
  .insert({
    id: user.id,
    full_name: 'John Doe',
    email: user.email,
  })
```

### Update
```typescript
const { data, error } = await supabase
  .from('user_roadmap_progress')
  .update({ status: 'completed' })
  .eq('user_id', userId)
  .eq('node_id', nodeId)
```

### Join Tables
```typescript
const { data, error } = await supabase
  .from('workshops')
  .select(`
    *,
    workshop_registrations (
      user_id,
      status
    )
  `)
  .eq('workshop_registrations.user_id', userId)
```

---

## 🎮 Gamification Points

```typescript
const POINTS = {
  ROADMAP_NODE_COMPLETE: 10,
  ROADMAP_COMPLETE: 100,
  WORKSHOP_ATTEND: 50,
  CONTEST_PARTICIPATE: 30,
  CONTEST_WIN: 200,
  PROBLEM_SOLVE_EASY: 5,
  PROBLEM_SOLVE_MEDIUM: 15,
  PROBLEM_SOLVE_HARD: 30,
  POST_CREATE: 5,
  POST_UPVOTED: 2,
}
```

---

## 🔔 Real-time Subscriptions

```typescript
const channel = supabase
  .channel('notifications')
  .on(
    'postgres_changes',
    {
      event: 'INSERT',
      schema: 'public',
      table: 'notifications',
      filter: `user_id=eq.${userId}`,
    },
    (payload) => {
      console.log('New notification:', payload.new)
    }
  )
  .subscribe()
```

---

## 🎯 Performance Optimization Tips

1. **Use Server Components** by default
2. **Implement pagination** for large lists
3. **Add loading states** with Suspense
4. **Optimize images** with Next.js Image
5. **Cache API responses** with React Query
6. **Use database indexes** for frequent queries
7. **Implement lazy loading** for heavy components
8. **Minimize client-side JavaScript**

---

## 🐛 Common Issues & Solutions

### Issue: Supabase client not working
**Solution**: Check environment variables, ensure using correct client (browser vs server)

### Issue: TypeScript errors
**Solution**: Run `pnpm supabase gen types typescript` to regenerate types

### Issue: Slow page loads
**Solution**: Check for unnecessary client components, optimize database queries

### Issue: Authentication not persisting
**Solution**: Verify cookie settings, check middleware configuration

---

## 📚 Important Links

- **Development Plan**: `DEVELOPMENT_PLAN.md`
- **Tech Stack**: `TECH_STACK.md`
- **Getting Started**: `GETTING_STARTED.md`
- **Supabase Dashboard**: https://supabase.com/dashboard
- **Vercel Dashboard**: https://vercel.com/dashboard

---

## 🎓 Learning Resources

- [Next.js Docs](https://nextjs.org/docs)
- [Supabase Docs](https://supabase.com/docs)
- [Tailwind CSS Docs](https://tailwindcss.com/docs)
- [shadcn/ui](https://ui.shadcn.com)
- [React Query Docs](https://tanstack.com/query/latest)

---

## ✅ Pre-Launch Checklist

- [ ] All features tested
- [ ] Mobile responsive
- [ ] Accessibility audit (WCAG)
- [ ] Performance optimization (Lighthouse 90+)
- [ ] Security audit
- [ ] SEO optimization
- [ ] Error tracking setup (Sentry)
- [ ] Analytics setup (GA4)
- [ ] Backup strategy implemented
- [ ] Documentation complete
- [ ] User guide created
- [ ] Domain configured
- [ ] SSL certificate active
- [ ] Email service configured
- [ ] Monitoring setup

---

**Quick Start**: Run `pnpm dev` and start building! 🚀

