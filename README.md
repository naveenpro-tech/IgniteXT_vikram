# IgniteXT - Student Community Platform

> Empowering students through coding education, career guidance, and mentorship

[![Next.js](https://img.shields.io/badge/Next.js-14-black)](https://nextjs.org/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5-blue)](https://www.typescriptlang.org/)
[![Supabase](https://img.shields.io/badge/Supabase-PostgreSQL-green)](https://supabase.com/)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind-CSS-38B2AC)](https://tailwindcss.com/)

## 🚀 Mission

IgniteXT is a student-driven community platform with a mission to bring coding exposure, career guidance, and personal growth opportunities to every student. We organize workshops, coding contests, mentorship sessions, and provide comprehensive roadmaps for all tech stacks.

## ✨ Key Features

### 🎓 Learning & Development
- **Interactive Roadmaps** - Step-by-step learning paths for DSA, Web Dev, AI/ML, and more
- **Progress Tracking** - Track your learning journey with detailed analytics
- **Practice Problems** - Solve coding problems with integrated code editor
- **Resource Library** - Access curated learning materials (PDFs, videos, articles)

### 👥 Community & Mentorship
- **1:1 Mentorship** - Book personalized sessions with experienced mentors
- **Community Forum** - Ask questions, share achievements, and help others
- **Workshops & Events** - Participate in live workshops and coding contests
- **Anonymous Support** - Get help with mental wellness and career concerns

### 🏆 Gamification
- **Badges & Achievements** - Earn badges for completing milestones
- **Leaderboards** - Compete with peers and track your ranking
- **Points System** - Gain points for learning activities and contributions

### 💼 Career Guidance
- **Career Explorer** - Explore different tech career paths
- **Skill Roadmaps** - Know what skills you need for your dream job
- **Success Stories** - Learn from alumni and industry professionals
- **Placement Support** - Get guidance for interviews and placements

### 🤖 AI-Powered Features
- **Intelligent Chatbot** - Get instant answers to your queries
- **Personalized Recommendations** - Receive tailored learning suggestions
- **Career Assistant** - AI-powered career guidance

## 📱 Progressive Web App

IgniteXT is built as a Progressive Web App (PWA), which means:
- ✅ Install on mobile devices like a native app
- ✅ Works offline with cached content
- ✅ Receive push notifications
- ✅ Fast loading and smooth performance

## 🛠️ Technology Stack

### Frontend
- **Next.js 14** - React framework with App Router
- **TypeScript** - Type-safe development
- **Tailwind CSS** - Utility-first styling
- **shadcn/ui** - Beautiful UI components

### Backend
- **Supabase** - Backend-as-a-Service
  - PostgreSQL database
  - Authentication (Email, Google, Phone)
  - Real-time subscriptions
  - File storage
  - Row Level Security

### Deployment
- **Vercel** - Frontend hosting
- **Supabase Cloud** - Database hosting
- **GitHub Actions** - CI/CD pipeline

## 📋 Project Status

### Phase 1: MVP ✅ (Completed)
- [x] User authentication
- [x] Interactive roadmaps
- [x] Workshop registration
- [x] Contest participation
- [x] Basic chatbot

### Phase 2: Engagement 🚧 (In Progress)
- [ ] Progress tracking & gamification
- [ ] Mentorship booking system
- [ ] Community forum
- [ ] Resource library

### Phase 3: Expansion 📅 (Planned)
- [ ] Career options explorer
- [ ] Mental wellness support
- [ ] Admin dashboard
- [ ] AI-powered features
- [ ] PWA conversion

## 🚀 Getting Started

### Prerequisites
- Node.js 18+
- pnpm
- Git

### Installation

1. **Clone the repository**
```bash
git clone https://github.com/yourusername/ignitext.git
cd ignitext
```

2. **Install dependencies**
```bash
pnpm install
```

3. **Set up environment variables**
```bash
cp .env.local.example .env.local
# Edit .env.local with your Supabase credentials
```

4. **Run development server**
```bash
pnpm dev
```

5. **Open browser**
```
http://localhost:3000
```

For detailed setup instructions, see [GETTING_STARTED.md](GETTING_STARTED.md)

## 📚 Documentation

- **[Development Plan](DEVELOPMENT_PLAN.md)** - Comprehensive development roadmap
- **[Tech Stack](TECH_STACK.md)** - Detailed technology decisions
- **[Getting Started](GETTING_STARTED.md)** - Setup and installation guide
- **[Quick Reference](QUICK_REFERENCE.md)** - Quick reference for developers

## 🤝 Contributing

We welcome contributions from the community! Here's how you can help:

1. **Fork the repository**
2. **Create a feature branch** (`git checkout -b feature/amazing-feature`)
3. **Commit your changes** (`git commit -m 'Add amazing feature'`)
4. **Push to the branch** (`git push origin feature/amazing-feature`)
5. **Open a Pull Request**

### Development Guidelines
- Follow TypeScript best practices
- Write meaningful commit messages
- Add tests for new features
- Update documentation as needed
- Ensure code passes linting (`pnpm lint`)

## 🧪 Testing

```bash
# Run unit tests
pnpm test

# Run E2E tests
pnpm test:e2e

# Run linting
pnpm lint
```

## 📊 Project Structure

```
ignitext/
├── src/
│   ├── app/              # Next.js App Router pages
│   ├── components/       # React components
│   ├── lib/              # Utility functions
│   ├── hooks/            # Custom React hooks
│   ├── store/            # State management
│   └── types/            # TypeScript types
├── public/               # Static assets
├── supabase/             # Database migrations
└── tests/                # Test files
```

## 🎯 Roadmap

### Q1 2025
- ✅ MVP Launch
- ✅ User authentication
- ✅ Basic roadmaps

### Q2 2025
- 🚧 Mentorship system
- 🚧 Community forum
- 📅 Gamification

### Q3 2025
- 📅 Career explorer
- 📅 AI features
- 📅 Mobile app

### Q4 2025
- 📅 Advanced analytics
- 📅 Sponsor integrations
- 📅 Scale to 10,000+ users

## 📈 Success Metrics

- **Users**: 10,000+ registered students
- **Workshops**: 100+ workshops conducted
- **Mentorship**: 1,000+ sessions completed
- **Community**: 5,000+ forum posts
- **Roadmaps**: 50+ learning paths

## 🏆 Team

- **Developers**: Building the platform
- **Mentors**: Guiding students
- **Community Managers**: Fostering engagement
- **Content Creators**: Creating learning materials

## 📞 Contact

- **Website**: [ignitext.com](https://ignitext.com) (coming soon)
- **Email**: contact@ignitext.com
- **Discord**: [Join our community](https://discord.gg/ignitext)
- **Twitter**: [@ignitext](https://twitter.com/ignitext)

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- All contributors and community members
- Open source projects that made this possible
- Mentors who dedicate their time to help students
- Students who inspire us to build better

## 💡 Support

If you find IgniteXT helpful, please:
- ⭐ Star this repository
- 🐛 Report bugs and issues
- 💡 Suggest new features
- 📢 Share with your friends
- 🤝 Contribute to the project

---

**Built with ❤️ by the IgniteXT Team**

*Empowering the next generation of developers*

---

## 🔗 Quick Links

- [Live Demo](https://ignitext.vercel.app) (coming soon)
- [Documentation](./DEVELOPMENT_PLAN.md)
- [API Reference](./docs/API.md) (coming soon)
- [Contributing Guide](./CONTRIBUTING.md) (coming soon)
- [Code of Conduct](./CODE_OF_CONDUCT.md) (coming soon)

---

**Status**: 🚧 Under Active Development

**Version**: 0.1.0 (MVP)

**Last Updated**: 2025-09-30

