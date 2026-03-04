# FRAUDIX Deployment Guide

## Quick Deploy with Render + Vercel

### Prerequisites
- Render account (https://render.com)
- Vercel account (https://vercel.com)
- MongoDB Atlas cluster with connection URI
- Git repository pushed to GitHub

---

## Backend Deployment (Render)

### Step 1: Prepare Backend Repository
```bash
cd backend
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/YOUR_GITHUB_USERNAME/fraudix-backend.git
git push -u origin main
```

### Step 2: Deploy on Render
1. Go to [Render Dashboard](https://dashboard.render.com)
2. Click **"New +"** → **"Web Service"**
3. Connect your GitHub repository
4. Fill in the details:
   - **Name**: fraudix-backend
   - **Environment**: Node
   - **Build Command**: `npm install`
   - **Start Command**: `npm start`
   - **Publish Directory**: (leave empty)

5. Add Environment Variables:
   - **MONGO_URI**: Your MongoDB Atlas connection string
     ```
     mongodb+srv://username:password@cluster0.xxx.mongodb.net/fraudix
     ```

6. Click **"Create Web Service"**

### Result
Your backend will be live at: `https://fraudix-backend.onrender.com`

---

## Frontend Deployment (Vercel)

### Step 1: Prepare Frontend Repository
```bash
cd frontend
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/YOUR_GITHUB_USERNAME/fraudix-frontend.git
git push -u origin main
```

### Step 2: Deploy on Vercel
1. Go to [Vercel Dashboard](https://vercel.com/dashboard)
2. Click **"Add New..."** → **"Project"**
3. Import your **fraudix-frontend** GitHub repository
4. Configure:
   - **Framework**: None (Static HTML)
   - **Build Command**: (leave empty)
   - **Output Directory**: (leave empty)

5. Add Environment Variables:
   - **NEXT_PUBLIC_API_URL**: `https://fraudix-backend.onrender.com/api`

6. Click **"Deploy"**

### Result
Your frontend will be live at: `https://fraudix.vercel.app`

---

## Environment Configuration

### .env (Backend)
```env
MONGO_URI=mongodb+srv://username:password@cluster0.xxx.mongodb.net/fraudix
PORT=5000
```

### API URL in Frontend
The frontend automatically detects the environment:
- **Local**: `http://localhost:5000/api`
- **Production**: `https://fraudix-backend.onrender.com/api`

---

## Update Frontend API URL (if needed)

Edit [frontend/index.html](index.html) around line 1795:

```javascript
const API_URL = window.location.hostname === 'localhost' 
  ? 'http://localhost:5000/api'
  : 'https://fraudix-backend.onrender.com/api';
```

---

## Testing Deployment

### Backend Health Check
```bash
curl https://fraudix-backend.onrender.com/api/transactions
```

### Frontend
Visit: https://fraudix.vercel.app

### Test Login
- Create account with any username/password
- Login credentials are stored in MongoDB

---

## Troubleshooting

### Backend won't start
- Check MongoDB URI is correct
- Ensure credentials are in .env
- Check Render logs: Dashboard → Web Service → Logs

### Frontend not connecting to backend
- Verify backend URL in JavaScript console (F12 → Console)
- Check CORS is enabled in backend
- Ensure MongoDB connection is working

### CORS Errors
Add this to [backend/server.js](../backend/server.js):
```javascript
app.use(cors({
  origin: 'https://fraudix.vercel.app',
  credentials: true
}));
```

---

## Custom Domain (Optional)

### Vercel
1. Dashboard → Settings → Domains
2. Add custom domain
3. Follow DNS setup instructions

### Render
1. Dashboard → Settings → Custom Domain
2. Add domain
3. Update DNS records

---

## Monitoring & Logs

- **Render Logs**: Dashboard → Web Service → Logs
- **Vercel Logs**: Dashboard → Project → Deployments → Logs
- **MongoDB Atlas**: Cloud.mongodb.com → Metrics

---

## Need Help?

- Render Docs: https://render.com/docs
- Vercel Docs: https://vercel.com/docs
- MongoDB Atlas: https://docs.atlas.mongodb.com
