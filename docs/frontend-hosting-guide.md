# 🍿 Frontend Hosting Guide — Movies.snishad

This guide provides deep-dive instructions for deploying the frontend cinema web client on **Vercel**, **Netlify**, **Cloudflare Pages**, or **Docker**.

Repository: [snishad0198/movies-frontend](https://github.com/snishad0198/movies-frontend)  
Author: **SNishad** ([@snishad.me_](https://instagram.com/snishad.me_))

---

## 1. Hosting on Vercel (Recommended)

Vercel is the native platform for Next.js and provides zero-config edge caching, image optimization, and automatic SSL.

### Step-by-Step Instructions:
1. Sign up or log in to [Vercel](https://vercel.com).
2. Click **Add New** ➔ **Project**.
3. Import the `movies-frontend` repository from your GitHub account.
4. Set the project configuration:
   - **Framework Preset:** `Next.js`
   - **Root Directory:** `./`
   - **Build Command:** `next build` (or leave default)
   - **Output Directory:** `.next` (or leave default)
5. Add the following **Environment Variables**:
   | Variable | Value Description | Example Value |
   | :--- | :--- | :--- |
   | `NEXT_PUBLIC_SITE_NAME` | Name of your streaming site | `Movies.snishad` |
   | `NEXT_PUBLIC_SITE_URL` | Public production URL | `https://movies-snishad.vercel.app` |
   | `NEXT_PUBLIC_BACKEND_URL` | Live URL of your Backend API | `https://api.moviessnishad.com` |
   | `BACKEND_INTERNAL_URL` | Server-to-server Backend API URL | `https://api.moviessnishad.com` |
   | `INTERNAL_API_KEY` | Secure token shared with backend | `movies-snishad-secure-internal-token-2026` |
6. Click **Deploy**. Within 60 seconds, your site is globally accessible.

---

## 2. Custom Domain Configuration (Vercel)

1. Open your project on Vercel and navigate to **Settings** ➔ **Domains**.
2. Enter your custom domain (e.g. `watch.yourdomain.com` or `yourdomain.com`).
3. Add the displayed DNS records to your domain registrar (GoDaddy, Namecheap, Cloudflare):
   - **A Record:** `@` points to `76.76.21.21`
   - **CNAME Record:** `www` points to `cname.vercel-dns.com`
4. SSL certificates are provisioned automatically.

---

## 3. Self-Hosted Docker Deployment

If you want to host on your own Linux server:
```bash
# 1. Clone frontend repository
git clone https://github.com/snishad0198/movies-frontend.git
cd movies-frontend

# 2. Build Docker image
docker build -t movies-frontend .

# 3. Run container
docker run -d \
  --name movies_frontend_app \
  -p 3001:3001 \
  -e NEXT_PUBLIC_SITE_NAME="Movies.snishad" \
  -e NEXT_PUBLIC_SITE_URL="https://yourdomain.com" \
  -e NEXT_PUBLIC_BACKEND_URL="https://api.yourdomain.com" \
  -e BACKEND_INTERNAL_URL="https://api.yourdomain.com" \
  -e INTERNAL_API_KEY="movies-snishad-secure-internal-token-2026" \
  movies-frontend
```
