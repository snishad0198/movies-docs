# 📖 Movies.snishad — Official Platform Guide, Hosting Docs & Usage Policy

Welcome to the central documentation and hosting hub for **Movies.snishad**, an ultra-fast, modern open-source movie and web series streaming platform featuring automated TMDB synchronization, multi-server streaming, direct cloud downloads, personal watchlist, and an Apple HIG-inspired administration console.

Engineered and maintained by **SNishad** ([@snishad.me_](https://instagram.com/snishad.me_)).

---

## 🌐 Official Platform Ecosystem

| Component | Repository | Description | Primary Technology |
| :--- | :--- | :--- | :--- |
| **🍿 Frontend Client** | [snishad0198/movies-frontend](https://github.com/snishad0198/movies-frontend) | High-speed Cinema Web Client with Watchlist & Player | Next.js 14, Tailwind CSS, Framer Motion |
| **⚡ Backend API & Admin** | [snishad0198/movies-backend](https://github.com/snishad0198/movies-backend) | REST APIs, Hourly TMDB Sync & Apple Admin Console | Next.js 14, Prisma ORM, MySQL |
| **📦 All-in-One Monorepo** | [snishad0198/movies-web](https://github.com/snishad0198/movies-web) | Monorepo with 1-Command Full-Stack Runner | Complete Monorepo Setup |
| **📖 Documentation Hub** | [snishad0198/movies-docs](https://github.com/snishad0198/movies-docs) | Complete Deployment, Hosting & Licensing Guide | Markdown Docs & Docker Compose |

---

## 🔴 Live Demos & Default Credentials

* **Frontend Cinema Streaming Web:** [https://movies-snishad.vercel.app](https://movies-snishad.vercel.app) *(or your deployed domain)*
* **Backend Admin Console:** [https://your-backend-api.com/admin/login](https://your-backend-api.com/admin/login) *(or `http://localhost:3000/admin/login` locally)*
  * **Admin Email:** `admin@moviessnishad.com`
  * **Admin Password:** `Admin@123456`

---

## ⚖️ Usage Policy & Commercial Terms

Please review the usage terms below before cloning, hosting, or distributing this platform:

### 🟢 1. Personal & Educational Use (100% Free)
You are completely free to:
- Clone, study, customize, and experiment with the code for personal learning.
- Host the platform locally or for your personal/private family viewing.
- Use it as an educational reference or portfolio study.

### 🔴 2. Commercial Use & Business Monetization (Contact Required)
You may **NOT** under any circumstances:
- Monetize the platform through advertisements, subscriptions, paywalls, or donation gates without an official commercial license.
- Re-sell, re-package, or offer this platform as a paid SaaS or client solution without authorization.
- Remove copyright notices or author attribution to **SNishad**.

#### How to Obtain a Commercial License:
If you represent a business or wish to launch a commercial streaming brand, contact **SNishad**:
* **Instagram:** [@snishad.me_](https://instagram.com/snishad.me_)
* **Email:** `snishad01985@gmail.com`

---

## 🚀 How to Host Both Frontend & Backend

You have multiple easy hosting paths depending on your infrastructure preference:

### Option A: The Modern Cloud Stack (Vercel + Railway / Render / VPS)

#### Step 1: Host the Backend API
1. Fork or clone [snishad0198/movies-backend](https://github.com/snishad0198/movies-backend).
2. Deploy on **Railway**, **Render**, or your own **Ubuntu VPS**:
   * **Database:** Connect any MySQL database (e.g., Aiven Free MySQL, PlanetScale, Railway MySQL, or local MySQL).
   * **Environment Variables:**
     ```env
     DATABASE_URL="mysql://username:password@host:port/database"
     JWT_SECRET="movies-snishad-super-secret-jwt-key-min-32-chars-2026"
     INTERNAL_API_KEY="movies-snishad-secure-internal-token-2026"
     ALLOWED_FRONTEND_ORIGINS="https://your-frontend-domain.vercel.app"
     NEXT_PUBLIC_SITE_URL="https://your-backend-domain.com"
     ADMIN_EMAIL="admin@moviessnishad.com"
     ADMIN_PASSWORD="Admin@123456"
     ```
   * **Build Command:** `npm run build`
   * **Start Command:** `npx prisma db push && npm run start -p 3000`
3. Log in to `/admin/settings` and enter your free TMDB API key.

---

#### Step 2: Host the Frontend on Vercel (Takes 60 Seconds!)
1. Go to [Vercel](https://vercel.com) and click **"Add New Project"**.
2. Select repository [snishad0198/movies-frontend](https://github.com/snishad0198/movies-frontend).
3. Set **Framework Preset** to `Next.js`.
4. Enter your environment variables:
   ```env
   NEXT_PUBLIC_SITE_NAME="Movies.snishad"
   NEXT_PUBLIC_SITE_URL="https://your-frontend-domain.vercel.app"
   NEXT_PUBLIC_BACKEND_URL="https://your-backend-api-domain.com"
   BACKEND_INTERNAL_URL="https://your-backend-api-domain.com"
   INTERNAL_API_KEY="movies-snishad-secure-internal-token-2026"
   ```
5. Click **Deploy**! Vercel will build and deploy the frontend across global edge CDNs.

---

### Option B: 1-Command Docker Hosting (Recommended for VPS / Dedicated Server)

Anyone with Docker installed on Ubuntu/Debian/CentOS can launch the entire ecosystem in 1 single command:

```bash
# 1. Clone this documentation & deployment repository
git clone https://github.com/snishad0198/movies-docs.git
cd movies-docs

# 2. Run Docker Compose
docker compose up -d
```

This spins up:
1. `movies_mysql`: MySQL 8.0 server with persistent volume data.
2. `movies_backend`: Backend API & Admin Console running on port `3000`.
3. `movies_frontend`: Cinema Web Client running on port `3001`.

---

## 🔑 How to Setup Your Free TMDB API Key

1. Register for free at [themoviedb.org](https://www.themoviedb.org/).
2. Navigate to **Settings** ➔ **API** and create a Developer API Key.
3. Configure the key:
   * **Admin UI (Recommended):** Go to `https://your-backend-domain.com/admin/settings`, paste the key into the **"TMDB API & Sync"** section, click **"Test Connection"**, and click **Save**.
   * **Environment Variable:** Set `TMDB_API_KEY="your_key"` in backend `.env`.

---

## 💬 Creator, Bug Reports & Custom Website Builds

* **Creator:** **SNishad**
* **Instagram:** [@snishad.me_](https://instagram.com/snishad.me_)
* **Email:** `snishad01985@gmail.com`

> **Found a bug or issue?**  
> Reach out to me on Instagram ([@snishad.me_](https://instagram.com/snishad.me_)) with details so I can release a patch.

> **Need a custom website or web application built at an affordable price?**  
> Send an inquiry on Instagram ([@snishad.me_](https://instagram.com/snishad.me_)) or email `snishad01985@gmail.com`. Streaming platforms, custom dashboards, e-commerce, and APIs crafted with high-end designs.

---

## 📄 License
This repository is licensed under the [Commercial Restriction & Personal Use License](LICENSE).  
Free for Personal & Educational Use. For Commercial Use, contact **SNishad**.
