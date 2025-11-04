# Deployment Troubleshooting Guide

## Step-by-Step Deployment Process

### 1. Deploy Backend FIRST

**Using Vercel:**
1. Go to vercel.com
2. Click "Add New" → "Project"
3. Import `backend-campus-event-hub` repository
4. Configure:
   - **Root Directory**: Leave as is (already at root)
   - **Framework Preset**: Other
   - **Build Command**: Leave empty
   - **Output Directory**: Leave empty
5. **Environment Variables** (CRITICAL):
   ```
   MONGODB_URI=your_mongodb_atlas_connection_string
   JWT_SECRET=any_random_long_string_here
   NODE_ENV=production
   ```
6. Click "Deploy"
7. **COPY THE BACKEND URL** (e.g., `https://backend-campus-event-hub.vercel.app`)

### 2. Test Backend

Open in browser: `https://your-backend-url.vercel.app/api/health`

You should see:
```json
{
  "status": "ok",
  "message": "Server is running"
}
```

If this doesn't work, backend deployment failed.

### 3. Deploy Frontend

**Using Vercel:**
1. Import `frontend-campus-event-hub` repository
2. Configure:
   - **Root Directory**: Leave as is
   - **Framework Preset**: Vite
   - **Build Command**: `npm run build`
   - **Output Directory**: `dist`
3. **Environment Variables** (CRITICAL):
   ```
   VITE_API_URL=https://your-backend-url.vercel.app/api
   ```
   ⚠️ **IMPORTANT**: Use the EXACT backend URL from step 1, add `/api` at the end
4. Click "Deploy"

### 4. Debug Issues

#### Check Browser Console
1. Open deployed frontend site
2. Press F12 to open Developer Tools
3. Go to "Console" tab
4. Look for:
   - `API Base URL: https://...` - Should show your backend URL
   - Any red errors about CORS or network failures

#### Common Issues

**Issue 1: "API Base URL: http://localhost:5000/api"**
- **Problem**: Environment variable not set correctly
- **Solution**: 
  - Go to Vercel dashboard → Your frontend project → Settings → Environment Variables
  - Add `VITE_API_URL` with your backend URL
  - Redeploy (Important: Vercel needs a redeploy after adding env vars)

**Issue 2: CORS Error**
- **Problem**: Backend doesn't allow frontend domain
- **Solution**:
  - Go to backend Vercel project → Settings → Environment Variables
  - Add: `FRONTEND_URL=https://your-frontend-url.vercel.app`
  - Redeploy backend

**Issue 3: Network Error / Failed to fetch**
- **Problem**: Backend not deployed or MongoDB not connected
- **Solution**: Test backend health endpoint (see Step 2)

**Issue 4: Login keeps showing sign in page**
- **Problem**: API request is failing silently
- **Solution**: Check Network tab in DevTools
  - Look for failed requests to `/api/auth/login`
  - Check the error response

#### MongoDB Atlas Setup

If backend health check fails, it's likely MongoDB:

1. Go to MongoDB Atlas
2. Database Access → Add Database User (save username/password)
3. Network Access → Add IP Address → Allow access from anywhere (0.0.0.0/0)
4. Get connection string:
   - Clusters → Connect → Drivers
   - Copy connection string
   - Replace `<password>` with your database password
   - Replace `myFirstDatabase` with your database name (e.g., `campus-event-hub`)
5. Add to backend environment variables in Vercel

## Quick Checklist

- [ ] Backend deployed to Vercel
- [ ] Backend health endpoint works (`/api/health`)
- [ ] MongoDB Atlas configured with IP whitelist
- [ ] Frontend environment variable `VITE_API_URL` set correctly
- [ ] Frontend redeployed after adding environment variable
- [ ] Browser console shows correct API URL (not localhost)
- [ ] No CORS errors in browser console

## Still Not Working?

Share these details:
1. Backend Vercel URL
2. Frontend Vercel URL  
3. Screenshot of browser console (F12)
4. Screenshot of Network tab showing failed requests
