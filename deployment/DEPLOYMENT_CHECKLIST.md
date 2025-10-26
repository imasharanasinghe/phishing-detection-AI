# Deployment Checklist

Use this checklist when deploying to Render and Vercel.

## Pre-Deployment

- [ ] MongoDB Atlas account created
- [ ] MongoDB cluster created and accessible
- [ ] MongoDB connection string copied
- [ ] Firebase project created
- [ ] Firebase config copied
- [ ] GitHub repository is public (for easy deployment) OR connected to Render/Vercel
- [ ] All sensitive data removed from code
- [ ] config.env added to .gitignore ✓ (already done)

## Backend Deployment (Render)

### Step 1: Create Web Service
- [ ] Go to https://dashboard.render.com
- [ ] Click "New +" → "Web Service"
- [ ] Connect GitHub repository
- [ ] Select your repository

### Step 2: Configuration
- [ ] **Name**: phishing-detection-backend
- [ ] **Region**: Choose closest to users
- [ ] **Branch**: main (or master)
- [ ] **Root Directory**: `backend`
- [ ] **Runtime**: Python 3
- [ ] **Build Command**: `pip install -r requirements.txt`
- [ ] **Start Command**: `uvicorn app.main:app --host 0.0.0.0 --port $PORT`

### Step 3: Environment Variables
Add these in Render dashboard:

**Required:**
- [ ] `MONGODB_URL` = Your MongoDB connection string
- [ ] `DATABASE_NAME` = `phishing_ai`
- [ ] `SECRET_KEY` = Random 32+ character string
- [ ] `JWT_SECRET` = Random 32+ character string

**Optional (Recommended):**
- [ ] `ALLOWED_ORIGINS` = `*` (or specific domains)
- [ ] `DEBUG` = `False`
- [ ] `LOG_LEVEL` = `INFO`
- [ ] `FRONTEND_ORIGIN` = Your Vercel URL (after deployment)
- [ ] `GROQ_API` = Your Groq API key (if using)

**Firebase:**
- [ ] `FIREBASE_API_KEY` = Your Firebase API key
- [ ] `FIREBASE_AUTH_DOMAIN` = Your Firebase auth domain
- [ ] `FIREBASE_PROJECT_ID` = Your Firebase project ID
- [ ] `FIREBASE_STORAGE_BUCKET` = Your Firebase storage bucket
- [ ] `FIREBASE_MESSAGING_SENDER_ID` = Your messaging sender ID
- [ ] `FIREBASE_APP_ID` = Your Firebase app ID

### Step 4: Deploy
- [ ] Click "Create Web Service"
- [ ] Wait for deployment to complete (5-10 minutes)
- [ ] Copy backend URL: ________________________
- [ ] Test: `https://[your-backend-url]/api/health`

## Frontend Deployment (Vercel)

### Step 1: Create Project
- [ ] Go to https://vercel.com/dashboard
- [ ] Click "Add New..." → "Project"
- [ ] Import your GitHub repository

### Step 2: Configuration
- [ ] **Framework Preset**: Other
- [ ] **Root Directory**: `frontend`
- [ ] **Build Command**: (leave empty)
- [ ] **Output Directory**: (leave empty)

### Step 3: Deploy
- [ ] Click "Deploy"
- [ ] Wait for deployment to complete (2-5 minutes)
- [ ] Copy frontend URL: ________________________

## Post-Deployment Configuration

### Step 1: Update Frontend API
- [ ] Open your Vercel frontend
- [ ] Open browser console (F12)
- [ ] Run: `localStorage.setItem('API_BASE', 'https://[your-backend-url]')`
- [ ] Refresh page

### Step 2: Update Firebase
- [ ] Go to Firebase Console → Authentication
- [ ] Click "Settings" → "Authorized domains"
- [ ] Add your Vercel domain
- [ ] Save

### Step 3: Update MongoDB Atlas
- [ ] Go to MongoDB Atlas → Network Access
- [ ] Add IP Address: `0.0.0.0/0` (allow all)
- [ ] OR add Render IPs if you know them

### Step 4: Update Render Environment
- [ ] Go back to Render dashboard
- [ ] Edit Environment Variables
- [ ] Set `FRONTEND_ORIGIN` = Your Vercel URL
- [ ] Save changes (auto-redeploys)

## Testing

### Backend Tests
- [ ] Health check: https://[backend]/api/health
- [ ] API docs: https://[backend]/docs
- [ ] Test analyze endpoint

### Frontend Tests
- [ ] Homepage loads
- [ ] Can navigate to sign up
- [ ] Can navigate to sign in
- [ ] Demo mode works
- [ ] API calls succeed (check browser console)
- [ ] No CORS errors

### Integration Tests
- [ ] Sign up works
- [ ] Sign in works
- [ ] Email analysis works
- [ ] Dashboard loads
- [ ] History displays

## Monitoring

- [ ] Check Render logs regularly
- [ ] Check Vercel logs if issues arise
- [ ] Monitor MongoDB Atlas usage
- [ ] Set up error tracking (optional)

## Optional Optimizations

- [ ] Enable Render paid tier for faster cold starts ($7/month)
- [ ] Set up custom domain for frontend
- [ ] Set up custom domain for backend
- [ ] Configure CDN caching
- [ ] Set up monitoring/alerts

## Troubleshooting

If issues occur:
- [ ] Check deployment logs in Render
- [ ] Check deployment logs in Vercel
- [ ] Check browser console for errors
- [ ] Verify environment variables are set correctly
- [ ] Verify MongoDB is accessible
- [ ] Verify Firebase is configured correctly

## Success Criteria

✅ Backend responds to health checks
✅ Frontend loads without errors
✅ Can analyze emails through the UI
✅ Authentication works
✅ Data is stored in MongoDB
✅ No CORS errors in console

## Deployment Complete!

Once all items are checked, your application is live! 🎉

**Frontend URL**: https://[your-app].vercel.app
**Backend URL**: https://[your-app].onrender.com
**API Docs**: https://[your-app].onrender.com/docs
