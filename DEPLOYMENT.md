# 🚀 Deployment Guide - Vercel & Render

## Overview

- **Frontend**: Deploy to **Vercel** (static site)
- **Backend**: Deploy to **Render** (API server)
- **Database**: **MongoDB Atlas** (already configured)

---

## Part 1: Deploy Backend to Render

### Step 1: Create Render Account
1. Go to https://render.com
2. Sign up (free)
3. Connect GitHub account

### Step 2: Create Web Service
1. Click **"New +"** → **"Web Service"**
2. Connect your GitHub repository
3. Select your `project-hub` repo

### Step 3: Configure Service

**General Settings:**
- **Name**: `projecthub-backend`
- **Environment**: `Docker`
- **Region**: Choose closest to you (e.g., `us-east-1`)
- **Branch**: `main` (or your branch)

**Build Command:**
```
npm install
```

**Start Command:**
```
npm start
```

### Step 4: Set Environment Variables

In Render dashboard → **Environment**:

```env
MONGO_URI=mongodb+srv://5860vaibhav_db_user:BJInxCXRUcaNgVzK@projecthub.8pxxj1u.mongodb.net/projecthub?appName=projectHub
JWT_SECRET=pHub_s3cr3t_jwt_k3y_2026!xZ9
JWT_EXPIRE=7d
NODE_ENV=production
PORT=5000
CLIENT_URL=https://your-frontend-domain.vercel.app
SOCKET_IO_CORS=https://your-frontend-domain.vercel.app
```

### Step 5: Deploy

1. Click **"Create Web Service"**
2. Wait for build to complete (5-10 minutes)
3. Get your backend URL: `https://projecthub-backend-xxx.onrender.com`

---

## Part 2: Deploy Frontend to Vercel

### Step 1: Create Vercel Account
1. Go to https://vercel.com
2. Sign up (free)
3. Connect GitHub

### Step 2: Import Project
1. Click **"New Project"**
2. Select your `project-hub` repository
3. Click **"Import"**

### Step 3: Configure Build

**Framework Preset**: `Vite`

**Build Settings:**
- **Root Directory**: `client`
- **Build Command**: `npm run build`
- **Output Directory**: `dist`
- **Install Command**: `npm install`

### Step 4: Set Environment Variables

In Vercel dashboard → **Settings** → **Environment Variables**:

```env
VITE_API_URL=https://projecthub-backend-xxx.onrender.com/api
```

(Replace with your actual Render backend URL)

### Step 5: Deploy

1. Click **"Deploy"**
2. Wait for deployment (2-5 minutes)
3. Get your frontend URL: `https://project-hub-xxx.vercel.app`

---

## Part 3: Update Backend URL in Frontend

After frontend is deployed, update Render backend:

### In Render Dashboard:
1. Go to **Environment Variables**
2. Update `CLIENT_URL` and `SOCKET_IO_CORS`:

```env
CLIENT_URL=https://project-hub-xxx.vercel.app
SOCKET_IO_CORS=https://project-hub-xxx.vercel.app
```

3. Click **"Save"** → Service will redeploy

---

## Part 4: Test Deployment

### Frontend Tests:
```bash
# Visit your frontend URL
https://project-hub-xxx.vercel.app

# Check if it loads
# Try login/signup if available
```

### Backend Tests:
```bash
# Test health endpoint
https://projecthub-backend-xxx.onrender.com/api/health

# Should return success response
```

### Frontend → Backend Communication:
1. Open browser console (F12)
2. Check Network tab
3. Make an API request in the app
4. Verify request goes to correct backend URL

---

## Part 5: Environment Variables Summary

### Backend (Render)
```env
MONGO_URI=mongodb+srv://5860vaibhav_db_user:BJInxCXRUcaNgVzK@projecthub.8pxxj1u.mongodb.net/projecthub?appName=projectHub
JWT_SECRET=pHub_s3cr3t_jwt_k3y_2026!xZ9
JWT_EXPIRE=7d
NODE_ENV=production
PORT=5000
CLIENT_URL=https://project-hub-xxx.vercel.app
SOCKET_IO_CORS=https://project-hub-xxx.vercel.app
```

### Frontend (Vercel)
```env
VITE_API_URL=https://projecthub-backend-xxx.onrender.com/api
```

---

## Part 6: Troubleshooting

### Frontend loads but can't reach backend
**Issue**: CORS error or wrong API URL

**Fix**:
1. Check `VITE_API_URL` in Vercel env vars
2. Verify backend URL is correct in frontend code
3. Check Render backend `CLIENT_URL` and `SOCKET_IO_CORS`
4. Redeploy both services

### Backend returns 500 error
**Issue**: MongoDB connection or env vars

**Fix**:
1. Check Render logs: **Logs** tab
2. Verify `MONGO_URI` is correct
3. Check MongoDB Atlas whitelist includes Render IPs
4. Redeploy backend

### Slow initial load
**Issue**: Render free tier goes to sleep

**Fix**: Upgrade to paid tier or use cron job to keep alive

### Vercel 404 on routes
**Issue**: SPA routing not configured

**Fix**: Already configured in `nginx.conf` for Docker, verify `rewrites` in `vercel.json`

---

## Part 7: Monitoring

### View Render Logs:
- Dashboard → Select service → **Logs** tab
- Real-time error monitoring

### View Vercel Logs:
- Dashboard → Select project → **Deployments** tab
- Click deployment → **Logs**

### Monitor MongoDB:
- Go to MongoDB Atlas
- **Metrics** tab
- View connection stats

---

## Part 8: Custom Domain (Optional)

### Add Custom Domain to Vercel:
1. Settings → Domains
2. Add your domain
3. Update DNS records

### Add Custom Domain to Render:
1. Settings → Custom Domain
2. Add domain
3. Update DNS

---

## Part 9: Redeploy After Code Changes

### Push to GitHub:
```bash
git add .
git commit -m "Your changes"
git push origin main
```

### Auto-Deploy:
- **Vercel**: Auto-redeploys on push to main
- **Render**: Auto-redeploys on push to main (if configured)

---

## Quick Links

- **Render Dashboard**: https://dashboard.render.com
- **Vercel Dashboard**: https://vercel.com/dashboard
- **MongoDB Atlas**: https://cloud.mongodb.com
- **Backend URL Pattern**: `https://projecthub-backend-xxx.onrender.com`
- **Frontend URL Pattern**: `https://project-hub-xxx.vercel.app`

---

## Final Checklist

- [ ] Backend deployed to Render
- [ ] Frontend deployed to Vercel
- [ ] Environment variables set correctly
- [ ] Backend and frontend URLs updated
- [ ] Frontend loads successfully
- [ ] Backend API responds
- [ ] Frontend can reach backend
- [ ] MongoDB Atlas connection working
- [ ] No CORS errors
- [ ] Logs show no errors

---

**Deployment Complete!** 🎉

Your application is now live! Share your frontend URL with users.

For updates, just push to GitHub and services auto-redeploy. 🚀
