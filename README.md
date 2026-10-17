# 📖 Movies.snishad — Complete Platform Architecture & Hosting Master Guide

Welcome to the official master documentation for **Movies.snishad**. This guide provides an end-to-end explanation of **how the platform was engineered (kaise bana hai)** and **how to deploy & host both backend and frontend anywhere (host kaise karna hai)**.

Engineered and maintained by **SNishad** ([@snishad.me_](https://instagram.com/snishad.me_)).

---

## 🌐 Official Repositories

| Component | Repository Link | Description |
| :--- | :--- | :--- |
| **🍿 Frontend Client** | [snishad0198/movies-frontend](https://github.com/snishad0198/movies-frontend) | Next.js 14 Cinema Streaming Client with Watchlist & Player |
| **⚡ Backend API & Admin** | [snishad0198/movies-backend](https://github.com/snishad0198/movies-backend) | Next.js 14 REST API, Hourly TMDB Sync & Apple Admin Console |
| **📖 Documentation Hub** | [snishad0198/movies-docs](https://github.com/snishad0198/movies-docs) | Complete Platform Architecture & Deployment Guide |

---

## 🔴 Live Demos & Default Credentials

* **Frontend Streaming Web Demo:** [https://movies-snishad.vercel.app](https://movies-snishad.vercel.app) *(or your deployed Vercel URL)*
* **Backend Admin Console Demo:** `https://your-backend-api.com/admin/login` *(or `http://localhost:3000/admin/login` locally)*
  * **Email:** `admin@moviessnishad.com`
  * **Password:** `Admin@123456`

---

## 🏗️ 1. Platform Architecture — Ye Kaise Bana Hai?

Movies.snishad ko ek modern, decoupled full-stack architecture par banaya gaya hai jisse ye ultra-fast, secure aur easy-to-scale rahe:

```text
               +-------------------------------------------+
               |         End User / Web Browser            |
               +-------------------------------------------+
                                     |
                                     v
               +-------------------------------------------+
               |  🍿 FRONTEND CLIENT (Next.js 14 App Router)|
               |  - Ultra-fast global edge caching (Vercel)|
               |  - Cinema Video Player without overlays   |
               |  - Personal Watchlist / Bookmark Library  |
               |  - Framer Motion animations & dark UI     |
               |  - Shortcuts (/ or Ctrl+K instant search) |
               +-------------------------------------------+
                                     |
                          REST API Requests & Proxy
                                     |
                                     v
               +-------------------------------------------+
               |  ⚡ BACKEND SERVER (Next.js 14 + Prisma)  |
               |  - REST API Routes (/api/movies, trending)|
               |  - Apple HIG-inspired Admin Console       |
               |  - Multi-server streaming resolver        |
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

### Key Technical Decisions:
1. **Separation of Concerns:** Frontend aur Backend ko alag-alag repositories me rakha gaya hai taaki Frontend ko Vercel ke free global edge CDN par deploy kiya ja sake aur Backend ko kisi bhi cloud VPS ya Docker par run kiya ja sake.
2. **Zero-Exposure TMDB Security:** TMDB API keys ko GitHub ya `.env` me expose karne ke bajaye, admin panel `/admin/settings` se direct private database me store kiya jaata hai jaha real-time connection test button bhi diya gaya hai.
3. **Hourly Auto-Sync Engine:** Backend me ek in-memory caching layer banayi gayi hai jo har 1 ghante me TMDB ke trending API se latest box office releases auto-refresh karti hai.
4. **Optimistic Local Storage Watchlist:** Users bina kisi slow database authentication ke turant movies/series ko bookmark kar sakte hain jo `localStorage` me sync rehta hai aur Navbar me live count badge dikhata hai.
5. **No Overhead Video Player:** Player ke upar se saare irritating overlays aur laggy elements hata diye gaye hain taaki sub-millisecond playback start ho.

---

## 🚀 2. Complete Hosting Guide — Isko Host Kaise Karna Hai?

Platform ko host karna bahot aasan hai. Niche diye gaye 2 steps follow karein:

---

### Step 1: Backend API & Admin Console Host Karein

Backend ko aap **Ubuntu VPS**, **Railway**, **Render**, ya **Docker** par host kar sakte hain.

#### Method A: Ubuntu VPS / Cloud Server (PM2 + Nginx) — Recommended for VPS

1. **VPS me Node.js & MySQL install karein:**
   ```bash
   sudo apt update && sudo apt upgrade -y
   sudo apt install -y curl git nginx mysql-server
   curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
   sudo apt install -y nodejs
   sudo npm install -g pm2
   ```

2. **Backend repository clone karein:**
   ```bash
   git clone https://github.com/snishad0198/movies-backend.git /var/www/movies-backend
   cd /var/www/movies-backend
   npm install
   ```

3. **`.env` file banayein:**
   ```bash
   cp .env.example .env
   nano .env
   ```
   Apni details fill karein:
   ```env
   DATABASE_URL="mysql://root:YourPassword@localhost:3306/movies_snishad"
   JWT_SECRET="movies-snishad-super-secret-jwt-key-min-32-chars-2026"
   INTERNAL_API_KEY="movies-snishad-secure-internal-token-2026"
   ALLOWED_FRONTEND_ORIGINS="https://your-frontend-domain.vercel.app,http://localhost:3000"
   NEXT_PUBLIC_SITE_URL="https://api.yourdomain.com"
   ADMIN_EMAIL="admin@moviessnishad.com"
   ADMIN_PASSWORD="Admin@123456"
   ```

4. **Database schema push aur seed karein:**
   ```bash
   npx prisma db push
   npx prisma db seed
   ```

5. **Production build banakar PM2 se start karein:**
   ```bash
   npm run build
   pm2 start npm --name "movies-backend" -- run start -p 3000
   pm2 save
   pm2 startup
   ```

6. **Nginx Reverse Proxy & Free SSL (Let's Encrypt):**
   ```bash
   sudo nano /etc/nginx/sites-available/movies-backend
   ```
   Paste karein:
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
   Enable karein aur SSL lagayein:
   ```bash
   sudo ln -s /etc/nginx/sites-available/movies-backend /etc/nginx/sites-enabled/
   sudo nginx -t && sudo systemctl reload nginx
   sudo apt install certbot python3-certbot-nginx -y
   sudo certbot --nginx -d api.yourdomain.com
   ```

---

#### Method B: Railway / Render (Zero Server Management)
1. [Railway.app](https://railway.app) ya [Render.com](https://render.com) par free account banayein.
2. New **MySQL Database** create karein aur uska `DATABASE_URL` copy karein.
3. New **Web Service** create karein aur repository `snishad0198/movies-backend` connect karein.
4. Environment variables me `DATABASE_URL`, `JWT_SECRET`, aur `INTERNAL_API_KEY` add karein.
5. Build command: `npm run build`
6. Start command: `npx prisma db push && npm run start -p 3000`
7. Deploy ho jaane par aapko ek backend URL mil jayega (jaise `https://movies-backend.up.railway.app`).

---

### Step 2: Frontend Web Client Host Karein (Vercel — 60 Seconds)

Frontend ko Vercel par host karna 100% free hai aur sabse fast load hota hai:

1. [Vercel.com](https://vercel.com) par login karein.
2. Click **"Add New"** ➔ **"Project"**.
3. Repository **`snishad0198/movies-frontend`** select karein.
4. Project Settings me:
   * **Framework Preset:** `Next.js`
   * **Root Directory:** `./`
5. **Environment Variables** add karein:
   ```env
   NEXT_PUBLIC_SITE_NAME="Movies.snishad"
   NEXT_PUBLIC_SITE_URL="https://your-frontend-domain.vercel.app"
   NEXT_PUBLIC_BACKEND_URL="https://api.yourdomain.com"
   BACKEND_INTERNAL_URL="https://api.yourdomain.com"
   INTERNAL_API_KEY="movies-snishad-secure-internal-token-2026"
   ```
   *(Note: `NEXT_PUBLIC_BACKEND_URL` me apne Step 1 wale backend ka live URL dalein!)*
6. Click **"Deploy"**!  
   60 seconds ke andar aapka video streaming web application poori duniya me live ho jayega!

---

## 🔑 3. TMDB API Key Kaise Configure Karein? (1 Minute)

1. [themoviedb.org](https://www.themoviedb.org/) par free account banayein.
2. **Settings** ➔ **API** me jakar **Developer API Key** generate karein.
3. Apne backend ke admin panel me login karein: `https://api.yourdomain.com/admin/login`
   * **Email:** `admin@moviessnishad.com`
   * **Password:** `Admin@123456`
4. Left menu se **Settings** ➔ **"TMDB API & Sync"** tab open karein.
5. Apni TMDB API Key paste karein aur **"Test Connection"** click karein.
6. **"Save Settings"** par click karein. Ab aapke platform par trending movies, series, posters aur details dynamically fetch hona shuru ho jayengi!

---

## ⚖️ 4. Usage Policy & Commercial Terms

### 🟢 Personal & Educational Use (100% Free)
* Aap is project ko study karne, seekhne, college project, ya apne personal ghar ke use ke liye bilkul free me use kar sakte hain.
* Code ko modify karna aur customize karna completely allowed hai.

### 🔴 Commercial Use & Business Monetization (Contact Required)
* Agar aap is website par **Ads lagana chahte hain (Google AdSense, Popups, etc.)**, **users se payment/subscription lena chahte hain**, ya isko **kisi client ko bechna chahte hain**, toh iske liye commercial license lena anivarya hai.
* Author ki permission ke bina commercial monetization strictly prohibited hai.

### Commercial License Kaise Lein:
Directly contact karein:
* **Instagram:** [@snishad.me_](https://instagram.com/snishad.me_)
* **Email:** `snishad01985@gmail.com`

---

## 💬 Creator, Bug Reports & Freelance Work

* **Creator:** **SNishad**
* **Instagram:** [@snishad.me_](https://instagram.com/snishad.me_)
* **Email:** `snishad01985@gmail.com`

> **Koi bug mila?**  
> Direct Instagram [@snishad.me_](https://instagram.com/snishad.me_) par message karein, hum turant patch release karenge!

> **Apna website ya web app banwana chahte hain affordable price me?**  
> Instagram ya email par contact karein. High-performance streaming sites, e-commerce, aur modern web applications ready karke diye jaate hain.

---

## 📄 License
This repository is licensed under the [Commercial Restriction & Personal Use License](LICENSE).  
Free for Personal & Educational Use. For Commercial Use, contact **SNishad**.
