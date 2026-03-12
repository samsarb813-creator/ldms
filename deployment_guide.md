# Free Cloud Deployment Guide — LDMS

Since you want **100% Free** cloud hosting, here is the best way to do it. Standard providers like Heroku no longer have free tiers, so we will use a combination of two services.

## 🚀 The "Killer" Free Combo: Render + Aiven

This is the most reliable free setup for Laravel in 2024.

### 1. Get a Free MySQL Database (Aiven)
Render's free database (PostgreSQL) expires after 90 days. For **Forever Free MySQL**, use Aiven:
1. Sign up at [Aiven.io](https://aiven.io).
2. Create a **Free Plan** MySQL service.
3. Once active, copy the Service URI or Connection Details.

### 2. Deploy the App (Render)
1. Push your project to GitHub.
2. Go to [Render.com](https://render.com) and create a **New Web Service**.
3. Connect your GitHub repo.
4. Set **Environment**: `PHP`.
5. Set **Build Command**: 
   ```bash
   composer install --no-dev && php artisan optimize
   ```
6. Set **Start Command**: 
   ```bash
   php artisan serve --host 0.0.0.0 --port $PORT
   ```
7. Add these **Environment Variables**:
   - `APP_KEY`: (Your local .env key)
   - `DB_CONNECTION`: `mysql`
   - `DB_HOST`: (From Aiven)
   - `DB_PORT`: (From Aiven, usually 3306)
   - `DB_DATABASE`: `defaultdb`
   - `DB_USERNAME`: `avnadmin`
   - `DB_PASSWORD`: (From Aiven)
   - `APP_DEBUG`: `false`

---

## 🏗️ Option 2: AlwaysData (All-in-One Free)

[AlwaysData.com](https://www.alwaysdata.com) offers 100MB of free hosting that includes PHP, MySQL, and terminal access. It's more like traditional hosting but has a great free tier.

1. Sign up for a "Free" account.
2. Link your GitHub or upload via FTP.
3. Set up the MySQL database in their admin panel.

---

---

## 🔑 Environment Variables (Secrets)
When you deploy to Vercel, Railway, or Render, you must add these in the **Environment Variables** or **Secrets** section of their dashboard.

| Key | Value (What to write) |
| :--- | :--- |
| **APP_KEY** | `base64:IS3EhbT9lIRRPI8ZS5zMe4+DLLVlDBTZA0+DsJXZW1U=` |
| **APP_ENV** | `production` |
| **APP_DEBUG** | `false` |
| **DB_CONNECTION** | `mysql` |
| **DB_HOST** | (Get this from Aiven console) |
| **DB_PORT** | `3306` (or check Aiven) |
| **DB_DATABASE** | `defaultdb` |
| **DB_USERNAME** | `avnadmin` |
| **DB_PASSWORD** | (Get this from Aiven console) |

---

## ⚡ Option 3: Vercel (Fastest Frontend)

Vercel is great for performance, but requires a special configuration to run Laravel.

### 1. Preparation
1. I've created a `vercel.json` in your project root.
2. Ensure you have an **external MySQL database** (like Aiven.io) because Vercel does not have a database.

### 2. Connect to Vercel
1. Go to [Vercel.com](https://vercel.com) and click **Add New Project**.
2. Select your GitHub repository.
3. Vercel will detect it as a Laravel project or Other.
4. **Environment Variables**: Add your `APP_KEY`, `DB_HOST`, `DB_PASSWORD`, etc., in the Vercel Project Settings.

### 3. Key Points for Vercel
- Vercel uses a read-only filesystem. This means file uploads must go to external storage (like S3) or they will disappear.
- Logging should ideally go to `stderr`.

> [!IMPORTANT]
> Because Render/Railway free tiers "sleep" after inactivity, the first load might take 30-60 seconds. This is normal for free hosting!


