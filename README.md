# FRAME — "Every picture starts a conversation."

> FRAME is a visual social community combining the discovery of Pinterest, the depth of community discussions from Reddit, and the aesthetic animation quality of Apple.

---

## 📸 Key Features

1. **Editorial Landing Experience**:
   - Hero headline: *"Something caught your eye?"*
   - Tagline: *"Upload what you see. Discover what others notice. Start a conversation about anything."*
   - Dynamic floating photographic compositions, subtle parallax, glassmorphism, and smooth scroll transitions.

2. **Visual Discovery & Masonry Feeds**:
   - High-performance, responsive masonry grid layout with lazy-loaded imagery.
   - Dynamic feeds: **For You**, **Latest**, **Trending** (powered by engagement time-decay algorithms), and **Following**.
   - Category filtering: *Photography, Technology, Fashion, Art, Nature, Food, Travel, Objects, and Random*.
   - Debounced search across captions, user handles, categories, and `#tags`.

3. **Discussion & Signature Interaction**:
   - Signature **"What do you see?"** prompt encouraging diverse interpretations and stories.
   - Threaded hierarchical comment system with nested replies.
   - Likes and unlikes on comments.
   - Sorting by *Newest* or *Top Liked*.
   - Post owner deletion and inappropriate content reporting.

4. **Image Uploads & Validation**:
   - Drag-and-drop file upload interface with aspect-ratio calculation.
   - Strict client and server validation (JPEG, PNG, WebP with configurable 10 MB limit).
   - Real-time progress bar, cancellation, and publish confirmation toasts.
   - Persistent image storage in Supabase Storage (`frames` bucket) with local fallback support.

5. **Authentication & User Profiles**:
   - Supabase Auth (Email / Password registration, sign-in, sign-out, session persistence).
   - 1-click test creator switcher (Elena, Marcus, Maya, Sora) for instant offline demo evaluation.
   - Rich profiles with custom display names, unique usernames, avatars, bios, follower/following counts, shared frames grid, and bookmarks.
   - Follow/unfollow creator system.

6. **Safety, Moderation & Notifications**:
   - Content reporting modal (spam, harassment, explicit, copyright) routed to a built-in Safety Center queue.
   - Interactive notifications popover with unread indicators for likes, comments, replies, and new followers.
   - Strict Row Level Security (RLS) policies on all tables.

---

## 🛠️ Tech Stack

- **Frontend**: React 18, TypeScript, Vite
- **Styling**: Tailwind CSS (near-black `#090A0F` dark theme with electric-blue & violet accents, optional light mode)
- **Animations**: Motion for React (`framer-motion`)
- **Icons**: Lucide React
- **Backend & Database**: Supabase (PostgreSQL, Supabase Storage, Auth, Triggers, RLS)
- **Testing**: Vitest & React Testing Library

---

## 🚀 Quick Start Guide

### 1. Prerequisites
Ensure you have Node.js 18+ or 20+ installed.

### 2. Install Dependencies
```bash
npm install
```

### 3. Run the Development Server
```bash
npm run dev
```
Open [http://localhost:5173](http://localhost:5173) in your browser.

> **Note on Demo Mode**: Out of the box, FRAME works in **Interactive Local Mode** with persistent browser storage. You can upload frames, like, bookmark, comment, and test immediately without configuring Supabase first!

---

## 🗄️ Supabase Configuration & Migration

When you are ready to connect to your live Supabase Cloud project:

### Step 1: Create a Supabase Project
1. Go to [supabase.com](https://supabase.com) and create a new project.

### Step 2: Apply the Database Schema
1. In your Supabase dashboard, open the **SQL Editor**.
2. Copy and paste the entire contents of [`supabase/migrations/20250101000000_initial_schema.sql`](./supabase/migrations/20250101000000_initial_schema.sql).
3. Click **Run**. This will create:
   - `profiles`, `posts`, `comments`, `post_likes`, `comment_likes`, `bookmarks`, `follows`, `notifications`, `reports` tables.
   - Automatic triggers: profile creation on signup, real-time counters for likes and comments.
   - Row Level Security (RLS) policies for all tables.
   - Storage bucket `frames` with public read access and authenticated upload permissions.

### Step 3: Configure Environment Variables
1. Copy `.env.example` to `.env`:
   ```bash
   cp .env.example .env
   ```
2. In Supabase Dashboard -> **Project Settings** -> **API**, copy your `Project URL` and `anon public` key:
   ```env
   VITE_SUPABASE_URL=https://your-project.supabase.co
   VITE_SUPABASE_ANON_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpX...
   ```
3. Restart your dev server:
   ```bash
   npm run dev
   ```
   The navbar status pill will now display **"Supabase Live"** with a glowing green indicator.

---

## 🧪 Running Automated Tests

Run the test suite powered by Vitest:
```bash
npm test
```

Run TypeScript verification:
```bash
npm run lint
```

Build for production:
```bash
npm run build
```

---

## 🌐 Deployment Instructions

### Deploy to Vercel
1. Push your repository to GitHub or GitLab.
2. In the [Vercel Dashboard](https://vercel.com), click **Add New Project** and import the repository.
3. Configure the build settings:
   - **Framework Preset**: Vite
   - **Build Command**: `npm run build`
   - **Output Directory**: `dist`
4. In **Environment Variables**, add:
   - `VITE_SUPABASE_URL` = your Supabase URL
   - `VITE_SUPABASE_ANON_KEY` = your Supabase anon key
5. Click **Deploy**.

### Deploy to Netlify
1. Connect your repository on [Netlify](https://netlify.com).
2. Set build settings:
   - **Build command**: `npm run build`
   - **Publish directory**: `dist`
3. Add `_redirects` file for client-side routing if required:
   ```
   /*    /index.html   200
   ```
4. Add the `VITE_SUPABASE_URL` and `VITE_SUPABASE_ANON_KEY` environment variables.
5. Deploy site.

---

## 🛡️ Security & Row Level Security (RLS) Architecture

FRAME implements a zero-trust database security model:
- **Private Key Isolation**: No `service_role` keys are ever exposed in client code.
- **Strict RLS**: Users can only edit or delete their own posts, comments, and profile information.
- **Integrity Constraints**: Database-level unique constraints prevent duplicate likes, duplicate bookmarks, and users following themselves.
- **Moderation**: Reported content is logged into the `reports` table for moderator audit and swift removal.
