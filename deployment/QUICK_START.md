# Quick Start Deployment Guide

## What Changed

To deploy this application on Render (backend) and Vercel (frontend), the following changes were made:

### Files Created
1. `backend/runtime.txt` - Specifies Python version for Render
2. `deployment/ENVIRONMENT_SETUP.md` - Complete deployment guide
3. `deployment/QUICK_START.md` - This file

### Files Modified
1. `deployment/render.yaml` - Updated with correct configuration
2. `deployment/vercel.json` - Enhanced with rewrites and headers
3. `backend/app/main.py` - Updated CORS to support all origins in production

## Quick Deployment Steps

### 1. Backend on Render (5 minutes)

```bash
1. Go to https://dashboard.render.com
2. New + → Web Service
3. Connect GitHub repo
4. Configure:
   - Name: phishing-detection-backend
   - Root Directory: backend
   - Environment: Python 3
   - Build Command: pip install -r requirements.txt
   - Start Command: uvicorn app.main:app --host 0.0.0.0 --port $PORT
5. Add Environment Variables:
   - MONGODB_URL (from MongoDB Atlas)
   - DATABASE_NAME=phishing_ai
   - SECRET_KEY (generate random 32+ chars)
   - JWT_SECRET (generate random 32+ chars)
   - ALLOWED_ORIGINS=*
6. Deploy and copy the URL
```

### 2. Frontend on Vercel (3 minutes)

```bash
1. Go to https://vercel.com/dashboard
2. Add New → Project
3. Import GitHub repo
4. Configure:
   - Framework Preset: Other
   - Root Directory: frontend
5. Deploy
6. Copy the Vercel URL
```

### 3. Update Configuration (2 minutes)

After both deploy, update frontend to point to backend:

**Option 1: Add to frontend/index.html (before </head>)**
```html
<script>
  window.API_BASE = 'https://your-backend.onrender.com';
</script>
```

**Option 2: Update localStorage in browser console**
```javascript
localStorage.setItem('API_BASE', 'https://your-backend.onrender.com');
```

### 4. Update Firebase (2 minutes)

```bash
1. Firebase Console → Authentication → Settings
2. Add your Vercel domain to authorized domains
3. Save
```

## Important Notes

⚠️ **Render Free Tier:**
- Spins down after 15 minutes of inactivity
- First request after inactivity takes 30+ seconds
- Upgrade to $7/month for always-on instance

⚠️ **Environment Variables:**
- Never commit `config.env` to Git
- Set all secrets in Render/Vercel dashboard
- MongoDB Atlas must allow all IPs (0.0.0.0/0) for Render

⚠️ **CORS:**
- Backend now allows all origins by default
- For production, set `ALLOWED_ORIGINS` to specific domains

## Testing Deployment

1. Visit your Vercel frontend URL
2. Open browser console (F12)
3. Try signing up or demo mode
4. Check for API errors
5. Verify backend at: `https://your-backend.onrender.com/api/health`
6. Check API docs at: `https://your-backend.onrender.com/docs`

## Need Help?

See `deployment/ENVIRONMENT_SETUP.md` for detailed instructions and troubleshooting.

## Estimated Cost

- **Render**: Free (or $7/month for always-on)
- **Vercel**: Free
- **MongoDB Atlas**: Free (512MB)
- **Firebase**: Free
- **Total**: $0/month on free tier
