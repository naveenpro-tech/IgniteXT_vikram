# Getting Started with IgniteXT Development

This guide will help you set up the IgniteXT development environment and start building the platform.

## Prerequisites

Before you begin, ensure you have the following installed:

- **Node.js 18+** - [Download](https://nodejs.org/)
- **pnpm** - Install with: `npm install -g pnpm`
- **Git** - [Download](https://git-scm.com/)
- **VS Code** (Recommended) - [Download](https://code.visualstudio.com/)

## Step 1: Project Initialization

### 1.1 Create Next.js Project

```bash
# Create new Next.js project
pnpm create next-app@latest ignitext

# When prompted, select:
# ✅ TypeScript
# ✅ ESLint
# ✅ Tailwind CSS
# ✅ src/ directory
# ✅ App Router
# ❌ Turbopack (not yet stable)
# ✅ Import alias (@/*)

cd ignitext
```

### 1.2 Install Core Dependencies

```bash
# UI Components
pnpm add @radix-ui/react-dialog @radix-ui/react-dropdown-menu @radix-ui/react-select @radix-ui/react-tabs @radix-ui/react-toast
pnpm add class-variance-authority clsx tailwind-merge lucide-react

# State Management
pnpm add zustand @tanstack/react-query

# Forms & Validation
pnpm add react-hook-form @hookform/resolvers zod

# Supabase
pnpm add @supabase/supabase-js @supabase/ssr

# Utilities
pnpm add date-fns

# Dev Dependencies
pnpm add -D @types/node @types/react @types/react-dom
```

### 1.3 Set Up shadcn/ui

```bash
# Initialize shadcn/ui
pnpm dlx shadcn-ui@latest init

# When prompted:
# Style: Default
# Base color: Slate
# CSS variables: Yes

# Add commonly used components
pnpm dlx shadcn-ui@latest add button
pnpm dlx shadcn-ui@latest add input
pnpm dlx shadcn-ui@latest add card
pnpm dlx shadcn-ui@latest add dialog
pnpm dlx shadcn-ui@latest add dropdown-menu
pnpm dlx shadcn-ui@latest add form
pnpm dlx shadcn-ui@latest add toast
pnpm dlx shadcn-ui@latest add tabs
pnpm dlx shadcn-ui@latest add avatar
pnpm dlx shadcn-ui@latest add badge
```

## Step 2: Supabase Setup

### 2.1 Create Supabase Project

1. Go to [supabase.com](https://supabase.com)
2. Sign up / Log in
3. Click "New Project"
4. Fill in:
   - **Name**: ignitext
   - **Database Password**: (generate strong password)
   - **Region**: Choose closest to your users
5. Wait for project to be created (~2 minutes)

### 2.2 Get Supabase Credentials

1. Go to Project Settings > API
2. Copy:
   - **Project URL** (NEXT_PUBLIC_SUPABASE_URL)
   - **anon public** key (NEXT_PUBLIC_SUPABASE_ANON_KEY)
   - **service_role** key (SUPABASE_SERVICE_ROLE_KEY) - Keep this secret!

### 2.3 Configure Environment Variables

Create `.env.local` file in project root:

```bash
# Supabase
NEXT_PUBLIC_SUPABASE_URL=your_project_url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_anon_key
SUPABASE_SERVICE_ROLE_KEY=your_service_role_key

# App
NEXT_PUBLIC_APP_URL=http://localhost:3000
```

Create `.env.local.example` (for version control):

```bash
# Supabase
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_ANON_KEY=
SUPABASE_SERVICE_ROLE_KEY=

# App
NEXT_PUBLIC_APP_URL=
```

### 2.4 Set Up Supabase Client

Create `src/lib/supabase/client.ts`:

```typescript
import { createBrowserClient } from '@supabase/ssr'

export function createClient() {
  return createBrowserClient(
    process.env.NEXT_PUBLIC_SUPABASE_URL!,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!
  )
}
```

Create `src/lib/supabase/server.ts`:

```typescript
import { createServerClient, type CookieOptions } from '@supabase/ssr'
import { cookies } from 'next/headers'

export function createClient() {
  const cookieStore = cookies()

  return createServerClient(
    process.env.NEXT_PUBLIC_SUPABASE_URL!,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!,
    {
      cookies: {
        get(name: string) {
          return cookieStore.get(name)?.value
        },
      },
    }
  )
}
```

### 2.5 Create Database Schema

1. Go to Supabase Dashboard > SQL Editor
2. Create a new query
3. Copy the schema from `DEVELOPMENT_PLAN.md` (Section 3: Database Schema)
4. Run the query to create tables

Or use Supabase CLI:

```bash
# Install Supabase CLI
pnpm add -D supabase

# Initialize Supabase
pnpm supabase init

# Link to your project
pnpm supabase link --project-ref your_project_ref

# Create migration
pnpm supabase migration new initial_schema

# Add schema to migration file in supabase/migrations/
# Then push to database
pnpm supabase db push
```

## Step 3: Project Structure Setup

Create the following folder structure:

```bash
mkdir -p src/app/\(auth\)/{login,signup}
mkdir -p src/app/\(dashboard\)/{dashboard,profile,roadmaps,workshops,contests,mentorship,community,resources}
mkdir -p src/app/\(admin\)/admin/{dashboard,events,users,mentors,sponsors}
mkdir -p src/app/\(public\)/{about,careers,contact}
mkdir -p src/app/api/{auth,chatbot,webhooks}
mkdir -p src/components/{ui,layout,features,shared}
mkdir -p src/lib/{supabase,utils}
mkdir -p src/hooks
mkdir -p src/store
mkdir -p src/types
```

## Step 4: Configure TypeScript

Update `tsconfig.json`:

```json
{
  "compilerOptions": {
    "target": "ES2017",
    "lib": ["dom", "dom.iterable", "esnext"],
    "allowJs": true,
    "skipLibCheck": true,
    "strict": true,
    "noEmit": true,
    "esModuleInterop": true,
    "module": "esnext",
    "moduleResolution": "bundler",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "jsx": "preserve",
    "incremental": true,
    "plugins": [
      {
        "name": "next"
      }
    ],
    "paths": {
      "@/*": ["./src/*"]
    }
  },
  "include": ["next-env.d.ts", "**/*.ts", "**/*.tsx", ".next/types/**/*.ts"],
  "exclude": ["node_modules"]
}
```

## Step 5: Configure ESLint & Prettier

### ESLint

Update `.eslintrc.json`:

```json
{
  "extends": [
    "next/core-web-vitals",
    "plugin:@typescript-eslint/recommended"
  ],
  "rules": {
    "@typescript-eslint/no-unused-vars": "warn",
    "@typescript-eslint/no-explicit-any": "warn"
  }
}
```

### Prettier

Create `.prettierrc`:

```json
{
  "semi": false,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "es5",
  "printWidth": 80,
  "arrowParens": "avoid"
}
```

Create `.prettierignore`:

```
node_modules
.next
out
dist
build
```

## Step 6: Set Up Git

```bash
# Initialize git (if not already done)
git init

# Create .gitignore (should already exist)
# Ensure it includes:
# .env.local
# .env*.local
# node_modules
# .next

# Create repository on GitHub
# Then add remote and push
git remote add origin https://github.com/yourusername/ignitext.git
git add .
git commit -m "Initial commit: Project setup"
git push -u origin main
```

## Step 7: Run Development Server

```bash
# Start development server
pnpm dev

# Open browser
# Navigate to http://localhost:3000
```

## Step 8: Verify Setup

Create a simple test page to verify everything works:

Update `src/app/page.tsx`:

```typescript
import { Button } from '@/components/ui/button'

export default function Home() {
  return (
    <main className="flex min-h-screen flex-col items-center justify-center p-24">
      <h1 className="text-4xl font-bold mb-8">IgniteXT</h1>
      <Button>Get Started</Button>
    </main>
  )
}
```

If you see the page with a styled button, setup is successful! ✅

## Next Steps

Now that your environment is set up, you can start building:

1. **Week 1-2**: Follow the tasks in the development plan
   - Implement authentication
   - Create layout components
   - Build user profile page

2. **Week 3-4**: Build roadmaps and homepage
3. **Week 5-6**: Implement workshops, contests, and chatbot

Refer to `DEVELOPMENT_PLAN.md` for detailed implementation steps.

## Useful Commands

```bash
# Development
pnpm dev              # Start dev server
pnpm build            # Build for production
pnpm start            # Start production server
pnpm lint             # Run ESLint
pnpm format           # Format with Prettier

# Supabase
pnpm supabase start   # Start local Supabase
pnpm supabase stop    # Stop local Supabase
pnpm supabase status  # Check status
pnpm supabase db push # Push migrations

# Testing (after setup)
pnpm test             # Run tests
pnpm test:watch       # Run tests in watch mode
pnpm test:e2e         # Run E2E tests
```

## Troubleshooting

### Port 3000 already in use
```bash
# Kill process on port 3000
lsof -ti:3000 | xargs kill -9

# Or use different port
pnpm dev -p 3001
```

### Supabase connection issues
- Check environment variables are correct
- Verify Supabase project is running
- Check network/firewall settings

### TypeScript errors
```bash
# Clear Next.js cache
rm -rf .next

# Reinstall dependencies
rm -rf node_modules
pnpm install
```

## Resources

- [Next.js Documentation](https://nextjs.org/docs)
- [Supabase Documentation](https://supabase.com/docs)
- [Tailwind CSS Documentation](https://tailwindcss.com/docs)
- [shadcn/ui Documentation](https://ui.shadcn.com)
- [React Query Documentation](https://tanstack.com/query/latest)

## Need Help?

- Check `DEVELOPMENT_PLAN.md` for detailed implementation guidance
- Review `TECH_STACK.md` for technology decisions
- Create an issue on GitHub
- Contact the development team

---

**Ready to build IgniteXT!** 🚀

Start with Phase 1, Week 1-2 tasks and follow the development plan systematically.

