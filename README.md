# 📖 Movies.snishad — Platform Architecture, Engineering & Hosting Master Guide

Welcome to the official master documentation for **Movies.snishad**, a lightning-fast, production-ready, open-source movie and web series streaming platform featuring automated TMDB synchronization, multi-server streaming, direct cloud downloads, personal watchlist library, and an Apple HIG-inspired administration console.

Created and engineered by **SNishad** ([@snishad.me_](https://instagram.com/snishad.me_)).

---

## 🌐 Official Repositories

| Component | Repository Link | Description | Primary Technology |
| :--- | :--- | :--- | :--- |
| **🍿 Frontend Client** | [snishad0198/movies-frontend](https://github.com/snishad0198/movies-frontend) | Modern Cinema Web Client with Watchlist & Player | Next.js 14, Tailwind CSS, Framer Motion |
| **⚡ Backend API & Admin** | [snishad0198/movies-backend](https://github.com/snishad0198/movies-backend) | REST APIs, Hourly TMDB Sync & Apple Admin Console | Next.js 14, Prisma ORM, MySQL |
| **📖 Documentation Hub** | [snishad0198/movies-docs](https://github.com/snishad0198/movies-docs) | Complete Platform Architecture & Hosting Master Guide | Pure Markdown Documentation |

---

## 🔴 Live Demos & Default Credentials

* **Frontend Cinema Streaming Web Demo:** [https://movies-snishad.vercel.app](https://movies-snishad.vercel.app) *(or your deployed Vercel domain)*
* **Backend Admin Console Demo:** `https://your-backend-api.com/admin/login` *(or `http://localhost:3000/admin/login` locally)*
  * **Admin Email:** `admin@moviessnishad.com`
  * **Admin Password:** `Admin@123456`

---

## 🏗️ 1. Platform Architecture — How It Was Built

Movies.snishad is engineered with a modern, decoupled full-stack architecture to ensure sub-millisecond responsiveness, zero-overhead video playback, enterprise security, and effortless horizontal scalability:

```text
               +-------------------------------------------+
               |         End User / Web Browser            |
               +-------------------------------------------+
                                     |
                                     v
               +-------------------------------------------+
               |  🍿 FRONTEND CLIENT (Next.js 14 App Router)|
               |  - Ultra-fast global edge caching (Vercel)|
               |  - Clean Cinema Player with zero overlays |
               |  - Optimistic Watchlist / Bookmark Library|
               |  - Framer Motion micro-interactions & UI  |
               |  - Instant Search Shortcuts (/ or Ctrl+K) |
               +-------------------------------------------+
                                     |
                          REST API Requests & Proxy
                                     |
                                     v
               +-------------------------------------------+
               |  ⚡ BACKEND SERVER (Next.js 14 + Prisma)  |
               |  - REST API Endpoints (/api/movies, etc.) |
               |  - Apple HIG-inspired Admin Control Center|
               |  - Multi-server streaming node resolver   |
               |  - Direct Cloud 1080p/720p/480p proxy     |
               +-------------------------------------------+
                      /                              \
                     v                                v
     +-------------------------------+   +-----------------------------+
     |   🗄️ MySQL Database (Prisma)  |   | 🎬 TMDB API (TheMovieDB)    |
     |   - Movie / Series metadata   |   |   - Hourly trending catalog |
     |   - User comments & watchlist |   |   - High-res posters/backdrops
     |   - Encrypted admin settings  |   |   - Cast, crew & synopsis   |
     +-------------------------------+   +-----------------------------+
```

### Key Engineering Decisions:

1. **Decoupled Architecture (Micro-Frontend / Headless API):**
   * The Frontend and Backend are decoupled into separate repositories. This allows the Frontend to be distributed globally across Vercel's Edge CDN with zero server management, while the Backend runs on any standard VPS, Docker container, or cloud provider.

2. **Zero-Exposure TMDB Vault:**
   * Instead of committing sensitive API keys into code or public environment files, TMDB credentials are configured directly inside the database via the `/admin/settings` panel with a built-in real-time connection test button. Keys are never exposed to GitHub.

3. **Hourly Automated In-Memory Cache Engine:**
   * An intelligent in-memory caching layer was built into the backend. It automatically refreshes trending box office movies and series from TMDB every single hour with sub-5ms lookup latency, avoiding unnecessary external API rate limits.

4. **Optimistic Watchlist Library:**
   * Users can bookmark their favorite movies and series with 1 click. State is synchronized in `localStorage` with real-time browser events (`watchlist-updated`), instantly reflecting saved counts in both desktop and mobile navigation bars without requiring slow database round-trips.

5. **Lag-Free Cinema Player:**
   * Annoying overlays, fake pause buttons, and heavy tracking scripts have been eliminated. The player offers instant theater mode, seamless multi-server node switching, and direct high-speed download link proxying.

---

## 🚀 2. Complete Hosting Guide — How to Host Both Repositories

Deploying the entire platform is straightforward. Follow the two phases below:

---

### Phase 1: Deploying the Backend API & Admin Console

You can host the backend on an **Ubuntu VPS**, **Railway**, **Render**, or using **Docker**.

#### Option A: Ubuntu VPS (PM2 + Nginx + Free SSL) — Recommended for Production

1. **Install Node.js 20 LTS, Nginx, and MySQL Server:**
   ```bash
   sudo apt update && sudo apt upgrade -y
   sudo apt install -y curl git nginx mysql-server
   curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
   sudo apt install -y nodejs
   sudo npm install -g pm2
   ```

2. **Clone the Backend Repository:**
   ```bash
   git clone https://github.com/snishad0198/movies-backend.git /var/www/movies-backend
   cd /var/www/movies-backend
   npm install
   ```

3. **Configure Environment Variables (`.env`):**
   ```bash
   cp .env.example .env
   nano .env
   ```
   Fill in your production variables:
   ```env
   # MySQL Database Connection URL
   DATABASE_URL="mysql://root:YourPassword@localhost:3306/movies_snishad"

   # Secret Tokens (minimum 32 characters)
   JWT_SECRET="movies-snishad-super-secret-jwt-key-min-32-chars-2026"
   INTERNAL_API_KEY="movies-snishad-secure-internal-token-2026"

   # Allowed Frontend Origins (CORS Security)
   ALLOWED_FRONTEND_ORIGINS="https://your-frontend-domain.vercel.app,http://localhost:3000"
   NEXT_PUBLIC_SITE_URL="https://api.yourdomain.com"

   # Initial Admin Credentials
   ADMIN_EMAIL="admin@moviessnishad.com"
   ADMIN_PASSWORD="Admin@123456"
   ```

4. **Initialize Database Schema & Seeds:**
   ```bash
   npx prisma db push
   npx prisma db seed
   ```

5. **Build and Launch with PM2 Process Manager:**
   ```bash
   npm run build
   pm2 start npm --name "movies-backend" -- run start -p 3000
   pm2 save
   pm2 startup
   ```

6. **Configure Nginx Reverse Proxy & SSL (Certbot):**
   ```bash
   sudo nano /etc/nginx/sites-available/movies-backend
   ```
   Paste the following configuration:
   ```nginx
   server {
       server_name api.yourdomain.com;

       location / {
           proxy_pass http://localhost:3000;
           proxy_http_version 1.1;
           proxy_set_header Upgrade $http_upgrade;
           proxy_set_header Connection 'upgrade';
           proxy_set_header Host $host;
           proxy_cache_bypass $http_upgrade;
           proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
           proxy_set_header X-Forwarded-Proto $scheme;
       }
   }
   ```
   Enable the configuration and obtain a free Let's Encrypt SSL certificate:
   ```bash
   sudo ln -s /etc/nginx/sites-available/movies-backend /etc/nginx/sites-enabled/
   sudo nginx -t && sudo systemctl reload nginx
   sudo apt install certbot python3-certbot-nginx -y
   sudo certbot --nginx -d api.yourdomain.com
   ```

---

#### Option B: Cloud Serverless (Railway / Render / Docker)
1. Sign up on [Railway.app](https://railway.app) or [Render.com](https://render.com).
2. Create a new **MySQL Database** and copy the `DATABASE_URL`.
3. Create a new **Web Service** connected to repository `snishad0198/movies-backend`.
4. Enter your environment variables (`DATABASE_URL`, `JWT_SECRET`, `INTERNAL_API_KEY`, etc.).
5. Set Build Command: `npm run build`
6. Set Start Command: `npx prisma db push && npm run start -p 3000`
7. Once deployed, copy your live backend URL (e.g. `https://movies-backend.up.railway.app`).

---

### Phase 2: Deploying the Frontend Web Client (Vercel — 60 Seconds)

👉 **[Deploy to Vercel in 1-Click](https://vercel.com/new/clone?repository-url=https%3A%2F%2Fgithub.com%2Fsnishad0198%2Fmovies-frontend&env=NEXT_PUBLIC_SITE_NAME,NEXT_PUBLIC_SITE_URL,NEXT_PUBLIC_BACKEND_URL,BACKEND_INTERNAL_URL,INTERNAL_API_KEY)**

Deploying the frontend on Vercel is completely free and provides global CDN acceleration:

1. Log in to [Vercel](https://vercel.com).
2. Click **"Add New"** ➔ **"Project"**.
3. Select repository **`snishad0198/movies-frontend`**.
4. On the configuration screen:
   * **Framework Preset:** `Next.js`
   * **Root Directory:** `./`
5. Add the following **Environment Variables**:
   ```env
   NEXT_PUBLIC_SITE_NAME="Movies.snishad"
   NEXT_PUBLIC_SITE_URL="https://your-frontend-domain.vercel.app"
   NEXT_PUBLIC_BACKEND_URL="https://api.yourdomain.com"
   BACKEND_INTERNAL_URL="https://api.yourdomain.com"
   INTERNAL_API_KEY="movies-snishad-secure-internal-token-2026"
   ```
   *(Note: Set `NEXT_PUBLIC_BACKEND_URL` to your live backend domain from Phase 1!)*
6. Click **"Deploy"**!  
   In less than 60 seconds, your cinema streaming web application will be live globally!

---

## 🔑 3. Setting Up Your Free TMDB API Key (1 Minute)

1. Register for a free account at [themoviedb.org](https://www.themoviedb.org/).
2. Navigate to **Settings** ➔ **API** and request a free Developer API key.
3. Log in to your backend admin panel: `https://api.yourdomain.com/admin/login`
   * **Email:** `admin@moviessnishad.com`
   * **Password:** `Admin@123456`
4. From the sidebar, open **Settings** ➔ **"TMDB API & Sync"** tab.
5. Paste your TMDB API Key and click **"Test Connection"**.
6. Click **"Save Settings"**.  
   *Your platform will immediately begin streaming live posters, trending cinema, and box office details!*

---

## ⚖️ 4. Usage Policy & Commercial Terms

### 🟢 Personal & Educational Use (100% Free)
* You are completely free to clone, study, customize, and self-host this platform for personal entertainment or academic learning.
* Full code customization and local development are fully permitted.

### 🔴 Commercial Use & Business Monetization (Contact Required)
* If you plan to **run advertisements (Google AdSense, Popups, etc.)**, **charge users subscriptions or paywalls**, or **resell this platform to clients**, you are **strictly required to obtain a commercial license**.
* Commercial deployment without written authorization from the copyright holder is strictly prohibited.

### How to Request a Commercial License:
Contact **SNishad** directly:
* **Instagram:** [@snishad.me_](https://instagram.com/snishad.me_)
* **Email:** `snishad01985@gmail.com`

---

## 💬 Creator, Bug Reports & Freelance Inquiries

* **Created By:** **SNishad**
* **Instagram:** [@snishad.me_](https://instagram.com/snishad.me_)
* **Email:** `snishad01985@gmail.com`

> **Found a bug or issue?**  
> Send a message directly on Instagram ([@snishad.me_](https://instagram.com/snishad.me_)). Your bug reports help make this platform better for everyone!

> **Want a custom website or streaming app built at an affordable price?**  
> Reach out on Instagram or email. Custom streaming platforms, modern dashboards, e-commerce stores, and high-performance APIs built with cutting-edge UI/UX.

---

## 📄 License
This repository is licensed under the [Commercial Restriction & Personal Use License](LICENSE).  
Free for Personal & Educational Use. For Commercial Use, contact **SNishad**.
