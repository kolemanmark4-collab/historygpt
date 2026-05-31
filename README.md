# 📜 HistoryGPT — Full-Stack AI History Learning Platform

A full-stack SaaS application combining AI-powered historical research, chat, timelines, quizzes, and educational tools.

---

## 🏗️ Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | Next.js 14, React, TypeScript, Tailwind CSS |
| Backend | Node.js, Express.js |
| Database | PostgreSQL |
| Auth | Google OAuth 2.0 + Email/Password (Passport.js) |
| AI | Anthropic Claude API |
| Sessions | PostgreSQL-backed sessions |
| Export | PDFKit |

---

## 📁 Project Structure

```
historygpt/
├── frontend/          # Next.js 14 app
│   ├── src/
│   │   ├── app/
│   │   │   ├── page.tsx            # Redirects to /dashboard
│   │   │   ├── layout.tsx          # Root layout with fonts
│   │   │   ├── globals.css         # Global styles + CSS variables
│   │   │   ├── login/page.tsx      # Login (Google + Email)
│   │   │   ├── register/page.tsx   # Registration
│   │   │   ├── dashboard/page.tsx  # Main app (chat, timeline, research, quiz)
│   │   │   └── admin/page.tsx      # Admin dashboard
│   │   └── lib/
│   │       ├── api.ts              # All API calls (axios)
│   │       └── store.ts            # Zustand global state
│   ├── package.json
│   ├── tailwind.config.js
│   └── next.config.js
│
└── backend/           # Express.js API
    ├── src/
    │   ├── index.js               # Server entry point
    │   ├── db/
    │   │   ├── index.js           # PostgreSQL pool
    │   │   └── migrate.js         # Database migrations
    │   ├── config/
    │   │   └── passport.js        # Google OAuth + Local strategy
    │   ├── middleware/
    │   │   └── auth.js            # requireAuth, requireAdmin
    │   └── routes/
    │       ├── auth.js            # /api/auth/*
    │       ├── chats.js           # /api/chats/*
    │       ├── ai.js              # /api/ai/chat
    │       ├── notes.js           # /api/notes/*
    │       ├── admin.js           # /api/admin/*
    │       └── export.js          # /api/export/*
    ├── package.json
    └── .env.example
```

---

## 🚀 Local Development Setup

### 1. Prerequisites
- Node.js 18+
- PostgreSQL 14+
- Anthropic API key
- Google OAuth credentials

### 2. Clone & Install

```bash
# Install backend dependencies
cd historygpt/backend
npm install

# Install frontend dependencies
cd ../frontend
npm install
```

### 3. Set Up PostgreSQL

```bash
# Create database
psql -U postgres
CREATE DATABASE historygpt;
\q
```

### 4. Configure Environment Variables

**Backend** — copy `.env.example` to `.env`:
```bash
cd backend
cp .env.example .env
```

Edit `.env`:
```env
PORT=5000
NODE_ENV=development
FRONTEND_URL=http://localhost:3000
DATABASE_URL=postgresql://postgres:yourpassword@localhost:5432/historygpt
SESSION_SECRET=your-random-secret-here
GOOGLE_CLIENT_ID=your-google-client-id
GOOGLE_CLIENT_SECRET=your-google-client-secret
GOOGLE_CALLBACK_URL=http://localhost:5000/api/auth/google/callback
ANTHROPIC_API_KEY=sk-ant-your-key-here
```

**Frontend** — create `.env.local`:
```bash
cd frontend
echo "NEXT_PUBLIC_API_URL=http://localhost:5000" > .env.local
```

### 5. Run Database Migrations

```bash
cd backend
npm run db:migrate
```

### 6. Start Development Servers

```bash
# Terminal 1 — Backend
cd backend
npm run dev

# Terminal 2 — Frontend
cd frontend
npm run dev
```

Visit: **http://localhost:3000**

---

## ☁️ Deploy to Vercel + Railway

### Backend → Railway

1. Push code to GitHub
2. Go to [railway.app](https://railway.app) → New Project → Deploy from GitHub
3. Select the `backend` folder
4. Add a PostgreSQL database (Railway provides one)
5. Set environment variables in Railway dashboard (same as `.env`)
6. Railway auto-detects `npm start` and deploys

Your backend URL: `https://your-app.railway.app`

### Frontend → Vercel

1. Go to [vercel.com](https://vercel.com) → New Project → Import from GitHub
2. Set **Root Directory** to `frontend`
3. Add environment variable:
   - `NEXT_PUBLIC_API_URL` = your Railway backend URL
4. Deploy

---

## 🔑 Getting Your API Keys

### Google OAuth
1. Go to [console.cloud.google.com](https://console.cloud.google.com)
2. Create a project → APIs & Services → Credentials
3. Create OAuth 2.0 Client ID (Web application)
4. Add authorized redirect URIs:
   - Dev: `http://localhost:5000/api/auth/google/callback`
   - Prod: `https://your-backend.railway.app/api/auth/google/callback`

### Anthropic API Key
1. Go to [console.anthropic.com](https://console.anthropic.com)
2. API Keys → Create Key

---

## 🛠️ API Endpoints

### Auth
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/auth/google` | Initiate Google OAuth |
| GET | `/api/auth/google/callback` | OAuth callback |
| POST | `/api/auth/register` | Email registration |
| POST | `/api/auth/login` | Email login |
| POST | `/api/auth/logout` | Logout |
| GET | `/api/auth/me` | Get current user |

### Chats
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/chats` | Get all user chats |
| POST | `/api/chats` | Create new chat |
| GET | `/api/chats/:id` | Get chat + messages |
| PATCH | `/api/chats/:id` | Update chat (rename/bookmark) |
| DELETE | `/api/chats/:id` | Delete chat |
| POST | `/api/chats/:id/messages` | Add message to chat |

### AI
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/ai/chat` | Send message to Claude AI |

### Export
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/export/chat/:id/pdf` | Export chat as PDF |

### Admin (admin role required)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/admin/stats` | Platform statistics |
| GET | `/api/admin/users` | All users |
| PATCH | `/api/admin/users/:id/role` | Change user role |

---

## ✅ Features Checklist

- [x] Google OAuth sign-in
- [x] Email/password registration & login
- [x] Persistent chat history (PostgreSQL)
- [x] AI history chat (Claude API)
- [x] Detail levels (Simple / Standard / Academic)
- [x] Multiple modes (Chat, Timeline, Research, Quiz)
- [x] Chat renaming & deletion
- [x] PDF export of conversations
- [x] Admin dashboard with analytics
- [x] Rate limiting on AI endpoints
- [x] Session management (PostgreSQL-backed)
- [x] Responsive dark-mode UI

---

## 🔐 Security Features
- Helmet.js for HTTP headers
- Rate limiting (30 AI requests/min, 50 auth/15min)
- bcrypt password hashing (12 rounds)
- PostgreSQL session store
- HttpOnly, Secure cookies in production
- CORS restricted to frontend URL
