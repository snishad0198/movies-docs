# 📖 Movies.snishad — Official Platform Documentation & Hosting Hub

Welcome to the central documentation hub for **Movies.snishad**, a lightning-fast, modern open-source movie and web series streaming platform featuring automated TMDB synchronization, multi-server streaming, direct cloud downloads, personal watchlist, and an Apple HIG-inspired administration console.

Engineered and maintained by **SNishad** ([@snishad.me_](https://instagram.com/snishad.me_)).

---

## 🌐 Official Repositories

| Repository | Description | Primary Technology |
| :--- | :--- | :--- |
| **🍿 [snishad0198/movies-frontend](https://github.com/snishad0198/movies-frontend)** | High-speed Cinema Web Client with Watchlist & Player | Next.js 14, Tailwind CSS, Framer Motion |
| **⚡ [snishad0198/movies-backend](https://github.com/snishad0198/movies-backend)** | REST APIs, Hourly TMDB Sync & Apple Admin Console | Next.js 14, Prisma ORM, MySQL |
| **📖 [snishad0198/movies-docs](https://github.com/snishad0198/movies-docs)** | Central Deployment, Hosting & Licensing Documentation | Pure Markdown Documentation |

---

## 📚 Documentation Index

Explore our comprehensive guides:
* 🍿 [Frontend Hosting Guide (Vercel, Netlify, Custom Domains)](docs/frontend-hosting-guide.md)
* ⚡ [Backend Hosting Guide (VPS, Ubuntu, Nginx, PM2, Railway, Render)](docs/backend-hosting-guide.md)
* 🔑 [TMDB API Setup in 60 Seconds](docs/tmdb-setup-guide.md)
* ⚖️ [Commercial Licensing Policy & Terms](docs/commercial-licensing-policy.md)
* ❓ [Frequently Asked Questions (FAQ)](docs/faq.md)

---

## 🔴 Live Demos & Credentials

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

## 🚀 Quick Deployment Summary

### 1. Deploy the Backend (Ubuntu VPS / Railway / Render)
1. Clone [movies-backend](https://github.com/snishad0198/movies-backend).
2. Configure `.env` with your MySQL connection string and secret tokens.
3. Push database schema: `npx prisma db push && npx prisma db seed`.
4. Start server: `npm run build && npm run start -p 3000`.
5. Open `/admin/settings` to paste your free TMDB API key.
*(Read the complete [Backend Hosting Guide](docs/backend-hosting-guide.md) for Nginx & SSL setup).*

---

### 2. Deploy the Frontend (Vercel — Takes 60 Seconds)
1. Import [movies-frontend](https://github.com/snishad0198/movies-frontend) on [Vercel](https://vercel.com).
2. Set environment variables:
   ```env
   NEXT_PUBLIC_SITE_NAME="Movies.snishad"
   NEXT_PUBLIC_SITE_URL="https://your-frontend.vercel.app"
   NEXT_PUBLIC_BACKEND_URL="https://your-backend-api.com"
   BACKEND_INTERNAL_URL="https://your-backend-api.com"
   INTERNAL_API_KEY="movies-snishad-secure-internal-token-2026"
   ```
3. Click **Deploy**!
*(Read the complete [Frontend Hosting Guide](docs/frontend-hosting-guide.md) for custom domains).*

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
