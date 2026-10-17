# 🔑 TMDB API Setup Guide — Movies.snishad

This guide walks you through obtaining and setting up a free developer key from **The Movie Database (TMDB)** for automatic poster, backdrop, overview, and hourly trending synchronization.

Author: **SNishad** ([@snishad.me_](https://instagram.com/snishad.me_))

---

## Step 1: Create a Free TMDB Account
1. Open [themoviedb.org](https://www.themoviedb.org/).
2. Click **Sign Up** in the top right corner and verify your email address.

---

## Step 2: Request an API Key
1. Click your profile avatar in the top right and select **Settings**.
2. In the left sidebar, click **API**.
3. Under the **Request an API Key** section, click **Developer**.
4. Accept the terms of use.
5. Fill out the brief application form:
   - **Type of Application:** Website / Streaming
   - **Application Name:** Movies.snishad
   - **Application URL:** Your website URL or `http://localhost:3000`
   - **Summary:** Personal cinema streaming & trending catalog sync
6. Click **Submit**. Your **API Key (v3 auth)** will be generated instantly.

---

## Step 3: Configure the Key in Movies.snishad

You have two simple ways to configure the key:

### Method A: Admin Console (Recommended — Zero Git Leakage)
1. Log in to your backend admin panel at `/admin/settings`.
2. Open the **"TMDB API & Sync"** section.
3. Paste your TMDB v3 API Key into the field.
4. Click **"Test Connection"** to verify real-time connectivity with TMDB.
5. Click **"Save Settings"**.  
   *Your key is securely saved in the database with zero risk of accidentally leaking to GitHub.*

### Method B: Environment Variable
In your `backend/.env` file, set:
```env
TMDB_API_KEY="your_tmdb_api_key_here"
```
Restart your backend server to apply the changes.
