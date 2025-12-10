# Deployment Guide (MERN Stack)

This guide walks you through deploying your Full Stack Application using free and easy-to-use platforms.

**Recommended Services:**
- **Frontend**: [Vercel](https://vercel.com) (Best for React/Vite)
- **Backend**: [Render](https://render.com) (Free Node.js hosting)
- **Database**: [MongoDB Atlas](https://www.mongodb.com/atlas) (Free managed MongoDB)

---

## ⚠️ Important Note on File Uploads
Currently, your application stores uploaded images locally in the `server/public/uploads` folder.
- **On Free Tier Hosting (Render/Heroku/Vercel)**: The filesystem is "ephemeral". This means **uploaded images will disappear** whenever the server restarts or you deploy a new version.
- **For Production**: It is highly recommended to eventually migrate to a cloud storage service like **Cloudinary** or AWS S3 to ensure images are saved permanently.
- **For Now**: The app will work, but be aware that images may not persist long-term.

---

## Step 1: Push to GitHub
Ensure your code is pushed to a GitHub repository. Vercel and Render will pull the code directly from there.

---

## Step 2: Database (MongoDB Atlas)
1. Log in to [MongoDB Atlas](https://www.mongodb.com/atlas).
2. Create a new **Free Cluster** (M0 Sandbox).
3. Create a **Database User** (username/password) in the Security tab.
4. Allow access from **Anywhere (0.0.0.0/0)** in Network Access.
5. Get your **Connection String**:
   - Click "Connect" -> "Drivers".
   - It looks like: `mongodb+srv://<username>:<password>@cluster0.mongodb.net/?retryWrites=true&w=majority`
   - Replace `<password>` with your actual password.

---

## Step 3: Backend Deployment (Render)
1. Log in to [Render](https://dashboard.render.com/).
2. Click **New +** -> **Web Service**.
3. Connect your GitHub repository.
4. **Configuration**:
   - **Name**: `my-app-server` (or similar)
   - **Root Directory**: `server`
   - **Runtime**: `Node`
   - **Build Command**: `npm install`
   - **Start Command**: `node index.js`
   - **Instance Type**: `Free`
5. **Environment Variables** (Advanced section):
   - Key: `MONGO_URI`
   - Value: (Your MongoDB Connection String from Step 2)
6. Click **Create Web Service**.
7. Wait for deployment to finish. Copy the **Service URL** (e.g., `https://my-app-server.onrender.com`).

---

## Step 4: Frontend Deployment (Vercel)
1. Log in to [Vercel](https://vercel.com).
2. Click **Add New** -> **Project**.
3. Import your GitHub repository.
4. **Configure Project**:
   - **Framework Preset**: Vite (should be auto-detected)
   - **Root Directory**: Click "Edit" and select `client`.
5. **Environment Variables**:
   - Key: `VITE_API_URL`
   - Value: (Your Render Server URL from Step 3, e.g., `https://my-app-server.onrender.com`)
   - **Important**: Do NOT add a trailing slash `/` at the end.
6. Click **Deploy**.

---

## Developer Notes
- **Local Development**: A `.env` file has been created in `client/` with `VITE_API_URL=http://localhost:5000` so your app continues to work locally.
- **Code Changes**: I have updated your React components to use `import.meta.env.VITE_API_URL` instead of hardcoding `http://localhost:5000`.

