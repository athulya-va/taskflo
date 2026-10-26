# Final Project Submission Guide

> **Prepared for:** Athulya  
> **Course:** ICTAK Full Stack Application Development (ReactJS / MERN Stack)  
> **Submission Portal:** ICT Academy of Kerala Project Submission  

This guide provides the exact, step-by-step instructions to satisfy every item on the **Final Project Submission** portal:
1. **GitHub repository link** (publicly accessible)
2. **Hosted project link** (live deployment URL)
3. **Screenshots of the major client-side views of your application**
4. **Project report / documentation**

---

## 1. Creating Your GitHub Repository & Pushing Code

Follow these steps in your terminal (PowerShell or Git Bash):

### Step 1: Create a New Repository on GitHub
1. Open your browser and go to [github.com/new](https://github.com/new).
2. Repository name: `taskflo`.
3. Set visibility to **Public** *(Mandatory requirement by ICTAK)*.
4. **Do NOT** check "Add a README file" or ".gitignore" (these are already created in your project).
5. Click **Create repository**.
6. Copy the repository URL: `https://github.com/athulya-va/taskflo.git`.

### Step 2: Initialize & Push Code from Your Computer
Open **PowerShell** or your terminal, navigate to the project directory, and run:

```powershell
cd C:\Users\anils\.gemini\antigravity\scratch\task-management-system

# Initialize Git
git init

# Stage all files
git add .

# Create the initial commit
git commit -m "Milestone 1-9: Complete ICTAK MERN Stack Project - TaskFlow"

# Rename branch to main
git branch -M main

# Link to your GitHub repository
git remote add origin https://github.com/athulya-va/taskflo.git

# Push code to GitHub
git push -u origin main
```

---

## 2. Deploying Your Project (Free Hosted Links)

### Option A: Deploying Backend to Render (Free)
1. Go to [dashboard.render.com](https://dashboard.render.com/) and sign in with GitHub.
2. Click **New +** -> **Web Service**.
3. Select your repository `taskflo`.
4. Fill in:
   - **Name:** `taskflow-api-athulya`
   - **Root Directory:** `server`
   - **Environment:** `Node`
   - **Build Command:** `npm install`
   - **Start Command:** `node src/server.js`
5. In **Environment Variables**, add:
   - `NODE_ENV` = `production`
   - `PORT` = `5000`
   - `JWT_SECRET` = `a_very_long_secure_random_string_key_123`
   - `MONGODB_URI` = *(Your MongoDB Atlas connection string)*
   - `CLIENT_ORIGIN` = `*` (or your frontend Vercel URL)
6. Click **Deploy Web Service**. Once deployed, copy your API URL (e.g., `https://taskflow-api-athulya.onrender.com`).

### Option B: Deploying Frontend to Vercel (Free)
1. Go to [vercel.com](https://vercel.com/) and sign in with GitHub.
2. Click **Add New...** -> **Project**.
3. Import `taskflo`.
4. Configure Project:
   - **Root Directory:** click Edit and select `client`
   - **Framework Preset:** `Vite`
5. In **Environment Variables**, add:
   - `VITE_API_BASE_URL` = `https://taskflow-api-athulya.onrender.com/api`
6. Click **Deploy**.
7. Once deployed, you will get your live link: (e.g., `https://taskflow-client.vercel.app`).

---

## 3. Screenshots Checklist

The submission requires **screenshots of the major client-side views**. Create a folder named `screenshots/` and capture the following:

1. `01-dashboard-view.png` — Main dashboard displaying welcome banner, stat cards, and task grid.
2. `02-create-task-modal.png` — The modal form showing title, priority, status, and due date inputs.
3. `03-task-details-view.png` — Deep-link task details page with audit timestamps and metadata.
4. `04-login-register-view.png` — The authentication screen with validation and clean card design.
5. `05-mobile-responsive-view.png` — Mobile viewport showing responsive card layout.

---

## 4. Final Submission Form Checklist

When filling the submission portal form:

| Portal Field | What to Enter |
|---|---|
| **Project report / documentation** | Upload `ICTAK_INTERNSHIP_REPORT.md` (or export it to PDF using VS Code / Print to PDF). |
| **GitHub repository link** | `https://github.com/athulya-va/taskflo` (make sure it is Public). |
| **Hosted project link** | `https://taskflow-client.vercel.app` (or your chosen live URL). |
| **Screenshots of major views** | Attach the images from the `screenshots/` directory. |

---

*All files in this workspace are structured to give you full compliance with the ICTAK Full Stack Development guidelines.*
