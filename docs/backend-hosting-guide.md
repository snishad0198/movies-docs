# ⚡ Backend Hosting Guide — Movies.snishad

This guide explains how to host the backend API and admin console on **Ubuntu / VPS**, **Railway**, **Render**, or with **Docker**.

Repository: [snishad0198/movies-backend](https://github.com/snishad0198/movies-backend)  
Author: **SNishad** ([@snishad.me_](https://instagram.com/snishad.me_))

---

## 1. Hosting on Ubuntu VPS with PM2 & Nginx (Production Standard)

### Requirements:
- Ubuntu 20.04 or 22.04 LTS VPS (1 GB RAM minimum)
- Node.js 18+ or 20+ LTS
- MySQL 8.0 Server

### Step 1: Install System Dependencies
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y curl git nginx mysql-server

# Install Node.js 20 LTS
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install -y nodejs
sudo npm install -g pm2
```

### Step 2: Clone and Configure Backend
```bash
git clone https://github.com/snishad0198/movies-backend.git /var/www/movies-backend
cd /var/www/movies-backend

# Install dependencies
npm install

# Create .env file
nano .env
```

Paste your production `.env` configuration:
```env
DATABASE_URL="mysql://movies_user:StrongPassword@localhost:3306/movies_snishad"
JWT_SECRET="movies-snishad-super-secret-jwt-key-min-32-chars-2026"
INTERNAL_API_KEY="movies-snishad-secure-internal-token-2026"
ALLOWED_FRONTEND_ORIGINS="https://your-frontend-domain.vercel.app,http://localhost:3000"
NEXT_PUBLIC_SITE_URL="https://api.yourdomain.com"
ADMIN_EMAIL="admin@moviessnishad.com"
ADMIN_PASSWORD="YourStrongAdminPassword"
```

### Step 3: Run Database Migrations & Seeds
```bash
npx prisma db push
npx prisma db seed
```

### Step 4: Build and Start with PM2
```bash
npm run build
pm2 start npm --name "movies-backend" -- run start -p 3000
pm2 save
pm2 startup
```

### Step 5: Configure Nginx & SSL
Create an Nginx server block:
```bash
sudo nano /etc/nginx/sites-available/movies-backend
```
Add:
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
Enable the site and obtain free Let's Encrypt SSL:
```bash
sudo ln -s /etc/nginx/sites-available/movies-backend /etc/nginx/sites-enabled/
sudo nginx -t && sudo systemctl reload nginx
sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx -d api.yourdomain.com
```

---

## 2. Deploying on Railway or Render

1. Create a **MySQL Database** service on Railway or Render.
2. Create a new **Web Service** connected to repository `snishad0198/movies-backend`.
3. Add environment variables:
   - `DATABASE_URL`: Your cloud MySQL connection string.
   - `JWT_SECRET`: Random 32+ character string.
   - `INTERNAL_API_KEY`: Secret internal token.
   - `ALLOWED_FRONTEND_ORIGINS`: Your Vercel frontend URL.
4. Set Build Command: `npm run build`
5. Set Start Command: `npx prisma db push && npm run start -p 3000`
