# IgniteXT Platform - Deployment Guide

This guide provides step-by-step instructions for deploying the IgniteXT platform to production.

---

## 📋 Prerequisites

Before deploying, ensure you have:

1. **Vercel Account** - Sign up at [vercel.com](https://vercel.com)
2. **Supabase Project** - Already created (ID: fwwrowrsizsiyyhqvvga)
3. **Git Repository** - Code committed to a Git repository
4. **Domain Name** (Optional) - For custom domain setup

---

## 🚀 Deployment Steps

### 1. Prepare Environment Variables

Create a `.env.production` file with the following variables:

```bash
# Supabase Configuration
NEXT_PUBLIC_SUPABASE_URL=https://fwwrowrsizsiyyhqvvga.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_anon_key_here
SUPABASE_SERVICE_ROLE_KEY=your_service_role_key_here

# Application Configuration
NEXT_PUBLIC_APP_URL=https://your-domain.vercel.app
NODE_ENV=production
```

**Important:** Never commit `.env.production` to Git. Keep credentials secure.

---

### 2. Deploy to Vercel

#### Option A: Deploy via Vercel CLI

```bash
# Install Vercel CLI
npm install -g vercel

# Navigate to project directory
cd ignitext-app

# Login to Vercel
vercel login

# Deploy to production
vercel --prod
```

#### Option B: Deploy via Vercel Dashboard

1. Go to [vercel.com/new](https://vercel.com/new)
2. Import your Git repository
3. Select the `ignitext-app` directory as the root
4. Configure build settings:
   - **Framework Preset:** Next.js
   - **Build Command:** `pnpm build`
   - **Output Directory:** `.next`
   - **Install Command:** `pnpm install`
5. Add environment variables from `.env.production`
6. Click "Deploy"

---

### 3. Configure Supabase for Production

#### Update Authentication Settings

1. Go to Supabase Dashboard → Authentication → URL Configuration
2. Add your production URL to **Site URL:**
   ```
   https://your-domain.vercel.app
   ```
3. Add to **Redirect URLs:**
   ```
   https://your-domain.vercel.app/auth/callback
   https://your-domain.vercel.app/login
   https://your-domain.vercel.app/dashboard
   ```

#### Configure OAuth Providers

**Google OAuth:**
1. Go to [Google Cloud Console](https://console.cloud.google.com)
2. Add production URL to Authorized JavaScript origins
3. Add callback URL to Authorized redirect URIs:
   ```
   https://fwwrowrsizsiyyhqvvga.supabase.co/auth/v1/callback
   ```

---

### 4. Database Setup

#### Run Migrations (if any)

```bash
# Connect to Supabase
npx supabase link --project-ref fwwrowrsizsiyyhqvvga

# Push database changes
npx supabase db push
```

#### Seed Production Data

```bash
# Run seeding scripts
cd ignitext-app
npx tsx scripts/seed-database.ts
npx tsx scripts/seed-contests.ts
npx tsx scripts/seed-all-data.ts
```

---

### 5. Post-Deployment Verification

#### Test Critical Features

1. **Authentication:**
   - [ ] Email/password login
   - [ ] Google OAuth login
   - [ ] User registration
   - [ ] Password reset

2. **Core Features:**
   - [ ] Dashboard loads correctly
   - [ ] Roadmaps display and progress tracking
   - [ ] Workshop registration
   - [ ] Contest participation
   - [ ] Profile editing and avatar upload

3. **Phase 2 Features:**
   - [ ] Mentorship booking
   - [ ] Community posts and comments
   - [ ] Admin dashboard access
   - [ ] Resources library
   - [ ] Announcements display
   - [ ] Practice problems and code submission

4. **Performance:**
   - [ ] Page load times < 3 seconds
   - [ ] Images optimized and loading
   - [ ] No console errors
   - [ ] Mobile responsiveness

---

## 🔒 Security Checklist

- [ ] Environment variables properly configured
- [ ] Service role key not exposed in client code
- [ ] Row Level Security (RLS) enabled on all tables
- [ ] HTTPS enabled (automatic with Vercel)
- [ ] CORS configured correctly
- [ ] Rate limiting configured (if needed)
- [ ] Input validation on all forms
- [ ] SQL injection prevention (using Supabase client)

---

## 📊 Monitoring & Analytics

### Set Up Monitoring

1. **Vercel Analytics:**
   - Enable in Vercel Dashboard → Analytics
   - Track page views, performance, and errors

2. **Supabase Monitoring:**
   - Monitor database performance
   - Track API usage
   - Set up alerts for errors

3. **Error Tracking (Optional):**
   - Integrate Sentry for error tracking
   - Set up error notifications

---

## 🔄 Continuous Deployment

### Automatic Deployments

Vercel automatically deploys when you push to your Git repository:

- **Production:** Push to `main` branch
- **Preview:** Push to any other branch

### Manual Deployments

```bash
# Deploy specific branch
vercel --prod

# Deploy with specific environment
vercel --prod --env-file .env.production
```

---

## 🌐 Custom Domain Setup

1. Go to Vercel Dashboard → Settings → Domains
2. Add your custom domain
3. Configure DNS records:
   ```
   Type: A
   Name: @
   Value: 76.76.21.21

   Type: CNAME
   Name: www
   Value: cname.vercel-dns.com
   ```
4. Wait for DNS propagation (up to 48 hours)
5. SSL certificate automatically provisioned

---

## 🐛 Troubleshooting

### Common Issues

**Issue: Build fails on Vercel**
- Check build logs for errors
- Ensure all dependencies are in `package.json`
- Verify Node.js version compatibility

**Issue: Authentication not working**
- Verify redirect URLs in Supabase
- Check environment variables
- Ensure OAuth credentials are correct

**Issue: Database connection errors**
- Verify Supabase URL and keys
- Check RLS policies
- Ensure service role key is set

**Issue: Images not loading**
- Check Supabase Storage bucket permissions
- Verify image URLs
- Ensure CORS is configured

---

## 📈 Performance Optimization

### Recommended Optimizations

1. **Enable Caching:**
   ```javascript
   // next.config.js
   module.exports = {
     images: {
       domains: ['fwwrowrsizsiyyhqvvga.supabase.co'],
     },
   }
   ```

2. **Database Indexing:**
   - Indexes already created on frequently queried columns
   - Monitor slow queries in Supabase

3. **Image Optimization:**
   - Use Next.js Image component
   - Compress images before upload
   - Use WebP format when possible

4. **Code Splitting:**
   - Already implemented with Next.js App Router
   - Dynamic imports for heavy components

---

## 🔐 Backup & Recovery

### Database Backups

Supabase automatically creates daily backups:
- Go to Supabase Dashboard → Database → Backups
- Download backups for local storage
- Test restore process periodically

### Code Backups

- Code is version controlled with Git
- Push to remote repository regularly
- Tag releases for easy rollback

---

## 📞 Support & Maintenance

### Regular Maintenance Tasks

- [ ] Monitor error logs weekly
- [ ] Review database performance monthly
- [ ] Update dependencies quarterly
- [ ] Backup database monthly
- [ ] Review security settings quarterly

### Getting Help

- **Vercel Support:** [vercel.com/support](https://vercel.com/support)
- **Supabase Support:** [supabase.com/support](https://supabase.com/support)
- **Next.js Docs:** [nextjs.org/docs](https://nextjs.org/docs)

---

## ✅ Deployment Checklist

- [ ] Environment variables configured
- [ ] Vercel project created and deployed
- [ ] Supabase authentication URLs updated
- [ ] OAuth providers configured
- [ ] Database seeded with sample data
- [ ] All features tested in production
- [ ] Security checklist completed
- [ ] Monitoring set up
- [ ] Custom domain configured (optional)
- [ ] Backup strategy in place
- [ ] Documentation updated

---

**Deployment Date:** _____________
**Deployed By:** _____________
**Production URL:** _____________

---

*Last Updated: October 1, 2025*

