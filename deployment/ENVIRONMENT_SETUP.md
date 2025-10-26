# Deployment Guide for Render and Vercel

This guide explains how to deploy your Phishing Detection AI application to Render (backend) and Vercel (frontend).

## Prerequisites

1. **Render Account** (free tier available)
2. **Vercel Account** (free tier available)
3. **MongoDB Atlas Account** (free tier available)
4. **Firebase Account** (for authentication)
5. **GitHub Repository** (to connect to deployment platforms)

## Part 1: Backend Deployment on Render

### Step 1: Connect GitHub Repository
1. Go to [Render Dashboard](https://dashboard.render.com/)
2. Click "New +" → "Web Service"
3. Connect your GitHub repository
4. Select the repository containing this project

### Step 2: Configure Backend Service
1. **Name**: `phishing-detection-ai-backend`
2. **Root Directory**: `backend`
3. **Environment**: `Python 3`
4. **Build Command**: `pip install -r requirements.txt`
5. **Start Command**: `uvicorn app.main:app --host 0.0.0.0 --port $PORT`

### Step 3: Environment Variables
Add the following environment variables in Render dashboard:

**Required:**
```
MONGODB_URL=mongodb+srv://username:password@cluster.mongodb.net/?retryWrites=true&w=majority
DATABASE_NAME=phishing_ai
SECRET_KEY=your-secret-key-here-min-32-chars
JWT_SECRET=your-jwt-secret-min-32-chars
```

**Optional (but recommended):**
```
DEBUG=False
LOG_LEVEL=INFO
FRONTEND_ORIGIN=https://your-frontend-url.vercel.app
GROQ_API=your-groq-api-key
ALLOWED_ORIGINS=*
```

**Firebase (for authentication):**
```
FIREBASE_API_KEY=your-firebase-api-key
FIREBASE_AUTH_DOMAIN=your-app.firebaseapp.com
FIREBASE_PROJECT_ID=your-project-id
FIREBASE_STORAGE_BUCKET=your-app.appspot.com
FIREBASE_MESSAGING_SENDER_ID=123456789
FIREBASE_APP_ID=1:123456789:web:abc123
FIREBASE_CREDENTIALS_PATH=firebase-service-account.json
```

### Step 4: Deploy
1. Click "Create Web Service"
2. Wait for deployment to complete (5-10 minutes)
3. Note your backend URL: `https://your-app-name.onrender.com`

## Part 2: Frontend Deployment on Vercel

### Step 1: Prepare Frontend
1. Update API configuration in frontend files if needed
2. The frontend uses `localStorage` and `window.API_BASE` to determine backend URL

### Step 2: Connect to Vercel
1. Go to [Vercel Dashboard](https://vercel.com/dashboard)
2. Click "Add New..." → "Project"
3. Import your GitHub repository

### Step 3: Configure Project Settings
1. **Framework Preset**: Other
2. **Root Directory**: `./frontend`
3. **Build Command**: (leave empty, static site)
4. **Output Directory**: (leave empty)

### Step 4: Environment Variables (if using)
Add any frontend-specific environment variables:
```
VITE_API_BASE=https://your-backend-url.onrender.com
```

### Step 5: Deploy
1. Click "Deploy"
2. Wait for deployment to complete
3. Note your frontend URL: `https://your-app-name.vercel.app`

## Part 3: Update API Configuration

After deploying both services, update the frontend to point to the backend:

### Option 1: Update Frontend Build (Recommended)
Add this to your frontend HTML files before the closing `</head>` tag:

```html
<script>
  // Set API base URL for production
  window.API_BASE = 'https://your-backend-url.onrender.com';
  localStorage.setItem('API_BASE', 'https://your-backend-url.onrender.com');
</script>
```

### Option 2: Use Vercel Environment Variables
Set `API_BASE` as an environment variable in Vercel and access it in JavaScript.

## Part 4: Firebase Configuration

1. Go to Firebase Console → Project Settings
2. Add your Vercel domain to authorized domains
3. Update OAuth redirect URIs to include your Vercel domain

## Part 5: Chrome Extension Configuration

If using the Chrome extension:

1. Update `chrome_extension/popup.js` to set default API:
   ```javascript
   api.value = res.API_BASE || 'https://your-backend-url.onrender.com';
   ```

2. Update `manifest.json` if needed

3. Load unpacked extension in Chrome with the updated config

## Troubleshooting

### Backend Issues

**Problem**: Backend health check fails
- **Solution**: Check if MongoDB URI is correct and accessible from Render

**Problem**: Timeout on first request
- **Solution**: Render free tier has cold starts. First request may take 30+ seconds

**Problem**: CORS errors
- **Solution**: Set `ALLOWED_ORIGINS` environment variable or set to `*`

### Frontend Issues

**Problem**: API calls failing with 404
- **Solution**: Ensure API_BASE is set correctly and points to your Render backend

**Problem**: Firebase auth not working
- **Solution**: Add your Vercel domain to Firebase authorized domains

### Database Issues

**Problem**: MongoDB connection fails
- **Solution**: 
  - Check MongoDB Atlas IP whitelist (allow 0.0.0.0/0 for Render)
  - Verify connection string
  - Check database user permissions

## Security Considerations

1. **Never commit** `config.env` or `firebase-service-account.json` to Git
2. Use environment variables for all secrets
3. Enable MongoDB network security
4. Use HTTPS only (Render and Vercel provide this)
5. Set proper CORS origins in production (avoid using `*`)

## Cost Estimation

### Free Tier (Both Platforms)
- **Render**: Free tier with limitations (spins down after 15min inactivity)
- **Vercel**: Unlimited for frontend static sites
- **MongoDB Atlas**: Free tier with 512MB storage
- **Firebase**: Free tier with generous limits

### Recommended Paid Tiers (If Scaling)
- **Render**: $7/month for always-on instance
- **Vercel**: Free tier usually sufficient
- **MongoDB Atlas**: $9/month for better performance

## Support

For issues or questions:
1. Check Render logs in dashboard
2. Check Vercel deployment logs
3. Check browser console for frontend errors
4. Check backend API docs at `/docs` endpoint
