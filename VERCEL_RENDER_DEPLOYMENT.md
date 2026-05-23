# 🚀 Deployment Guide: Vercel Frontend + Render Backend

## Architecture Overview

```
┌─────────────────────────────────────────┐
│   Vercel (Frontend)                     │
│   https://your-app.vercel.app          │
│   - React app                           │
│   - Static hosting                      │
│   - Auto-deploys from GitHub            │
└──────────────────┬──────────────────────┘
                   │
                   │ API calls to
                   ▼
┌─────────────────────────────────────────┐
│   Render (Backend)                      │
│   https://your-backend.onrender.com     │
│   - FastAPI server                      │
│   - Python runtime                      │
│   - SQLite database                     │
│   - Auto-deploys from GitHub            │
└─────────────────────────────────────────┘
```

---

## Part 1: Deploy Backend to Render (10 minutes)

### Step 1.1: Prepare Render Account
1. Go to **https://render.com**
2. Click **Sign Up** (or **Login**)
3. Choose GitHub
4. Authorize Render

### Step 1.2: Create New Web Service
1. Click **"New +"** button
2. Click **"Web Service"**
3. Connect your GitHub repository:
   - Search for `gisul` (or your repo name)
   - Click **"Connect"**

### Step 1.3: Configure Web Service
Fill in the form:

| Field | Value |
|-------|-------|
| **Name** | `gisul-backend` |
| **Root Directory** | `backend-fastapi` |
| **Runtime** | `Python 3` |
| **Build Command** | `pip install -r requirements.txt` |
| **Start Command** | `gunicorn -w 4 -b 0.0.0.0:8000 main:app` |
| **Plan** | `Free` (or paid if needed) |

**Important:** Keep "Auto-deploy" ON

### Step 1.4: Add Environment Variables
Click **"Environment"** and add these variables:

```
SQLALCHEMY_DATABASE_URL = sqlite:///./gisul.db
SECRET_KEY = generate-a-strong-random-key-here
ALGORITHM = HS256
ACCESS_TOKEN_EXPIRE_MINUTES = 10080
PORT = 8000
```

**Generate SECRET_KEY at:** https://generate-random.org/ or use a UUID

### Step 1.5: Deploy
Click **"Create Web Service"** button

⏱️ **Wait 2-3 minutes for build**

You'll see green checkmark when done ✅

### Step 1.6: Get Backend URL
On the Render dashboard:
1. Open your service: `gisul-backend`
2. Copy the URL from top (looks like: `https://gisul-backend-xyz123.onrender.com`)
3. **Save this URL** - you'll need it for frontend

### Step 1.7: Test Backend
1. Go to: `https://your-backend-url/docs`
2. You should see **Swagger UI** with all API endpoints
3. Try **GET /health** endpoint

✅ **Backend is live!**

---

## Part 2: Deploy Frontend to Vercel (5 minutes)

### Step 2.1: Update Frontend with Backend URL
**Important:** Before deploying to Vercel, update the backend URL:

1. Go to: **frontend/.env.production**
2. Replace:
```
REACT_APP_API_URL=https://your-backend-url.onrender.com
```
(Use the URL from Step 1.6)

3. Save the file
4. **Commit and push to GitHub:**
```bash
git add frontend/.env.production
git commit -m "Update backend URL for production"
git push origin main
```

### Step 2.2: Create Vercel Account
1. Go to **https://vercel.com**
2. Click **Sign Up** (or **Login**)
3. Choose GitHub
4. Authorize Vercel

### Step 2.3: Import Frontend Project
1. Click **"Add New"** → **"Project"**
2. Click **"Continue with GitHub"**
3. Find `gisul` or your repo
4. Click **"Import"**

### Step 2.4: Configure Project
When you see the configuration screen:

| Setting | Value |
|---------|-------|
| **Framework** | Create React App |
| **Root Directory** | `frontend` |
| **Build Command** | `npm run build` |
| **Output Directory** | `build` |
| **Install Command** | (leave blank) |

### Step 2.5: Set Environment Variables
In the Environment Variables section, add:
```
REACT_APP_API_URL = https://your-backend-url.onrender.com
```
(Same URL as from Step 1.6)

### Step 2.6: Deploy
Click **"Deploy"** button

⏱️ **Wait 2-3 minutes**

✅ **Frontend is live!**

### Step 2.7: Get Frontend URL
You'll see your Vercel URL (like: `https://gisul-platform.vercel.app`)

---

## Part 3: Test Everything (5 minutes)

### Step 3.1: Test Frontend
1. Open your Vercel URL: `https://your-frontend.vercel.app`
2. You should see the Gisul login page

### Step 3.2: Test Login
1. Click **"Login with Demo Admin"** button
2. You should be logged in
3. You should see course catalog

### Step 3.3: Test Admin Features
1. Click **"Admin Dashboard"** in navbar
2. Try creating a new course
3. You should see success message

### Step 3.4: Test Enrollment
1. Click your name → **"Logout"**
2. Login as student (demo button)
3. Enroll in a course
4. Go to **"My Courses"** - you should see it

✅ **Everything works!**

---

## Demo Credentials

```
ADMIN ACCOUNT
Email:    admin@gisul.com
Password: admin123

STUDENT ACCOUNT
Email:    student@gisul.com
Password: student123
```

---

## Important URLs After Deployment

| Purpose | URL |
|---------|-----|
| **Frontend** | `https://your-app.vercel.app` |
| **Backend API** | `https://your-backend.onrender.com` |
| **API Docs** | `https://your-backend.onrender.com/docs` |
| **Health Check** | `https://your-backend.onrender.com/health` |

---

## Troubleshooting

### Problem: "API not responding" or CORS errors
**Solution:**
1. Check backend URL in `frontend/.env.production`
2. Make sure backend is running on Render (green status)
3. Test backend URL directly: `https://your-backend/docs`

### Problem: "Backend build failed on Render"
**Solution:**
1. Go to Render dashboard → Deployments
2. Click failed deployment
3. See error message at bottom
4. Common fixes:
   - Missing dependency in `requirements.txt`
   - Wrong Python version
   - Typo in environment variable name

### Problem: "Frontend build failed on Vercel"
**Solution:**
1. Go to Vercel dashboard → Deployments
2. Click failed build
3. See error in Logs tab
4. Common fixes:
   - Wrong environment variable
   - Missing backend URL
   - npm package missing

### Problem: "Cold start delay" (backend slow on first request)
**Solution:**
- This is normal for Render free tier
- App sleeps after 15 minutes of inactivity
- First request wakes it up (takes 30-60 seconds)
- Paid plans don't have this limitation

---

## Auto-Deployment on Changes

From now on, just push code to GitHub:
```bash
git add .
git commit -m "your changes"
git push origin main
```

Both Render and Vercel automatically:
1. ✅ Detect the push
2. ✅ Rebuild the project
3. ✅ Redeploy in 1-2 minutes
4. ✅ No manual action needed!

---

## File Cleanup

You can safely delete these files (they're not needed for this setup):

```
- api/                      (entire folder)
- DEPLOYMENT.md
- DEPLOYMENT_FILES.md
- DEPLOYMENT_READY.md
- DEPLOYMENT_STATUS.md
- DEPLOYMENT_STEP_BY_STEP.md
- VERCEL_DEPLOYMENT.md
- VERCEL_QUICKSTART.md
- docker-compose.yml
- Dockerfile
- Procfile
- railway.json
- deploy.sh
```

**Keep these:**
- `backend-fastapi/` (backend source)
- `frontend/` (frontend source)
- `render.yaml` (Render configuration)
- `vercel.json` (Vercel configuration)
- `.gitignore`
- `README.md`

---

## Updating Backend on Changes

If you modify backend code:
1. Push to GitHub:
```bash
cd backend-fastapi
git add .
git commit -m "backend changes"
git push origin main
```

2. Render automatically redeploys in 1-2 minutes ✅

## Updating Frontend on Changes

If you modify frontend code:
1. Push to GitHub:
```bash
cd frontend
git add .
git commit -m "frontend changes"
git push origin main
```

2. Vercel automatically redeploys in 1-2 minutes ✅

---

## Summary

| Component | Platform | URL Pattern | Auto-Deploy |
|-----------|----------|------------|------------|
| **Frontend** | Vercel | `*.vercel.app` | ✅ Yes |
| **Backend** | Render | `*.onrender.com` | ✅ Yes |
| **Database** | SQLite (local) | `./gisul.db` | N/A |

---

## Success Checklist

After both deployments:

- [ ] Backend URL is accessible (shows docs at /docs)
- [ ] Frontend URL is accessible (shows login page)
- [ ] Can login with demo credentials
- [ ] Can browse courses in catalog
- [ ] Admin can create courses
- [ ] Student can enroll in courses
- [ ] API calls work without CORS errors
- [ ] No console errors in browser

---

## Support Resources

- **Render Docs:** https://render.com/docs
- **Vercel Docs:** https://vercel.com/docs
- **FastAPI:** https://fastapi.tiangolo.com/
- **React:** https://react.dev/
- **GitHub:** Your repository

---

**You're all set! Both services will auto-deploy whenever you push to GitHub.** 🎉
