# TaskFlow — MERN Stack Task & Milestone Management System

> **Information & Communication Technology Academy of Kerala (ICTAK)**  
> **Full Stack Application Development Internship Project**  
> **Author / Student:** Athulya  
> **Repository:** https://github.com/athulya-va/taskflo  
> **Live Deployment:** *(Add your hosted URL here)*  

---

## 1. Executive Summary & Problem Statement

### Problem Statement
Modern project development and team collaboration suffer from scattered task tracking, lack of centralized status visibility, missed milestone deadlines, and unmanaged workloads. 

**TaskFlow** is a secure, responsive, full-stack MERN (MongoDB, Express.js, React, Node.js) web application designed to allow developers, students, and project teams to plan, organize, prioritize, and track their milestone deliverables in one cohesive dashboard with role-based access and real-time metric visualization.

### Target Users & Roles
- **Standard User (e.g. Student / Developer):** Can register, securely log in, create tasks, view own tasks with pagination and search, update task statuses, edit details, and track completion progress.
- **Administrator:** Can oversee milestone deliverables and review project statistics.

---

## 2. Project Architecture & Request Flow

The application follows the strict **3-Tier MERN Architecture** prescribed by ICTAK. The React client never communicates directly with the database; all database interactions are proxied and secured by the Node.js/Express REST API.

```
+-------------------+        HTTP (JSON)        +-------------------+        Queries        +-------------------+
|    React Client   | ------------------------> | Node/Express API  | --------------------> |      MongoDB      |
|    (Vite SPA)     | <------------------------ | (JWT Auth, REST)  | <-------------------- |   (Atlas / Local) |
+-------------------+        HTTP (JSON)        +-------------------+        Results        +-------------------+
```

### Request Flow (Example: Adding a New Task)
1. User completes the task form in React (Title, Category, Priority, Due Date).
2. React validates fields client-side and dispatches a `POST /api/tasks` request with `Authorization: Bearer <token>`.
3. Express server receives request; `cors` and `express.json` middlewares process payload.
4. `authMiddleware` validates JWT signature and loads authenticated user context (`req.user`).
5. `validationMiddleware` verifies data types and bounds on the server.
6. `taskController` invokes the Mongoose `Task` model to persist the document.
7. MongoDB saves record and responds; API returns `201 Created` with formatted JSON.
8. React updates application state and displays the newly created task without page refresh.

---

## 3. Project Structure

Following Section 4 (Separation of Concerns) of the ICTAK Student Reference Guide:

```
task-management-system/
├── client/                     # Vite + React Frontend Application
│   ├── public/                 # Static assets & favicon
│   ├── src/
│   │   ├── components/         # Reusable UI pieces (Navbar, TaskCard, Modal, etc.)
│   │   ├── context/            # Global Auth state context
│   │   ├── hooks/              # Custom hooks (useAuth, useTasks)
│   │   ├── pages/              # Route screens (Dashboard, Login, Register, TaskDetails, 404)
│   │   ├── services/           # Centralized API HTTP client
│   │   ├── App.jsx             # React Router route configuration
│   │   ├── App.css             # Responsive styling & themes
│   │   ├── index.css           # Global typography & CSS variables
│   │   └── main.jsx            # React root mount
│   ├── .env.example            # Client environment variables template
│   ├── index.html              # HTML5 entry point
│   ├── package.json            # Frontend dependencies
│   └── vite.config.js          # Vite build configuration
├── server/                     # Node.js & Express RESTful API
│   ├── src/
│   │   ├── config/             # MongoDB connection configuration
│   │   ├── controllers/        # Request handling & business logic
│   │   ├── middleware/         # Auth, input validation, and centralized error handling
│   │   ├── models/             # Mongoose schemas (User, Task)
│   │   ├── routes/             # RESTful endpoint definitions
│   │   └── server.js           # Server bootstrap & middleware pipeline
│   ├── .env.example            # Server environment variables template
│   └── package.json            # Backend dependencies
├── .gitignore                  # Git ignore rules (node_modules, .env, build output)
├── ICTAK_INTERNSHIP_REPORT.md  # Official Summer Internship Report documentation
├── SUBMISSION_GUIDE.md         # Step-by-step submission & deployment walkthrough
└── README.md                   # Comprehensive project documentation
```

---

## 4. API Contract & Endpoints

All endpoints are prefixed with `/api` and return standardized JSON responses.

### Authentication Endpoints (`/api/auth`)
| Method | Endpoint | Description | Access | Success Response | Error Codes |
|---|---|---|---|---|---|
| `POST` | `/api/auth/register` | Register new user account | Public | `201 Created` with token & user | 400, 409 |
| `POST` | `/api/auth/login` | Authenticate user & issue JWT | Public | `200 OK` with token & user | 400, 401 |
| `GET` | `/api/auth/me` | Fetch authenticated user profile | Private | `200 OK` with user details | 401, 404 |

### Task Management Endpoints (`/api/tasks`)
| Method | Endpoint | Description | Access | Success Response | Error Codes |
|---|---|---|---|---|---|
| `GET` | `/api/tasks` | Get paginated tasks with search & filters | Private | `200 OK` with tasks & paging | 401 |
| `POST` | `/api/tasks` | Create a new task | Private | `201 Created` with task object | 400, 401 |
| `GET` | `/api/tasks/stats` | Aggregated metrics for user dashboard | Private | `200 OK` with metric counts | 401 |
| `GET` | `/api/tasks/:id` | Fetch single task details | Private | `200 OK` with task | 401, 403, 404 |
| `PUT` | `/api/tasks/:id` | Update task details or status | Private | `200 OK` with updated task | 400, 401, 403, 404 |
| `DELETE` | `/api/tasks/:id` | Remove a task | Private | `200 OK` with deleted ID | 401, 403, 404 |

---

## 5. Screen States (4 UI States on Every Screen)

In strict compliance with **Section 7.3** of the ICTAK guidelines, all dynamic screens support:
1. **Loading State:** Clean CSS animated spinner rings indicating async API fetching.
2. **Success State:** Interactive grid of task cards with badges, priority indicators, and statistics.
3. **Empty State:** Friendly illustrations with clear prompts to create the first milestone task.
4. **Error State:** Alert banner displaying actionable, non-technical feedback with a "Try Again" retry button.

---

## 6. Local Setup and Installation Guide

### Prerequisites
- Node.js (v18.0.0 or higher)
- npm (v9.0.0 or higher)
- MongoDB (local service or MongoDB Atlas free cloud cluster)
- Git

### Step 1: Clone or Navigate to the Project
```bash
cd task-management-system
```

### Step 2: Configure Environment Variables

**Backend (`server/.env`):**
```env
PORT=5000
NODE_ENV=development
MONGODB_URI=mongodb://127.0.0.1:27017/taskmanager_ictak
JWT_SECRET=supersecret_ictak_mern_key_2026_dev_only
JWT_EXPIRE=7d
CLIENT_ORIGIN=http://localhost:5173
```

**Frontend (`client/.env`):**
```env
VITE_API_BASE_URL=http://localhost:5000/api
```

### Step 3: Install Dependencies
Open two terminal windows:

**Terminal 1 (Server):**
```bash
cd server
npm install
npm run dev
```
*(Server will start on http://localhost:5000)*

**Terminal 2 (Client):**
```bash
cd client
npm install
npm run dev
```
*(Client will start on http://localhost:5173)*

---

## 7. Git & GitHub Repository Instructions

To connect this project to your personal GitHub repository:

```bash
# 1. Initialize git in the root folder
git init

# 2. Stage all project files (safe: .gitignore excludes .env & node_modules)
git add .

# 3. Create your initial milestone commit
git commit -m "Milestone 1-9: Complete ICTAK MERN Stack Task Management System"

# 4. Set default branch to main
git branch -M main

# 5. Link your GitHub remote
git remote add origin https://github.com/athulya-va/taskflo.git

# 6. Push your project to GitHub
git push -u origin main
```

---

## 8. Deployment Walkthrough (Free Tier)

### 1. Database (MongoDB Atlas)
- Create a free Shared Cluster on [MongoDB Atlas](https://www.mongodb.com/cloud/atlas).
- Create a database user with password.
- Whitelist Network Access to `0.0.0.0/0` (Allow Access from Anywhere).
- Copy the connection string into the server's production environment variable `MONGODB_URI`.

### 2. Backend (Render / Railway)
- Connect your GitHub repo to [Render](https://render.com/).
- Set Root Directory to `server`.
- Build Command: `npm install`
- Start Command: `node src/server.js`
- Set Environment Variables: `PORT`, `NODE_ENV=production`, `MONGODB_URI`, `JWT_SECRET`, `CLIENT_ORIGIN`.

### 3. Frontend (Vercel)
- Connect your GitHub repo to [Vercel](https://vercel.com/).
- Set Root Directory to `client`.
- Framework Preset: `Vite`.
- Build Command: `npm run build`
- Output Directory: `dist`.
- Set Environment Variable: `VITE_API_BASE_URL=https://your-backend-api.onrender.com/api`.

---

## 9. Self-Review Checklist (Section 14 Compliance)

- [x] **Two-Sentence Pitch:** TaskFlow provides a secure, responsive MERN environment for users to plan, track, and complete milestone tasks. It solves project fragmentation through JWT-protected APIs, status updates, and metric dashboards.
- [x] **End-to-End Workflow:** Registration -> Login -> Token Store -> Dashboard Stats -> Add Task -> Filter/Search -> Edit/Status Toggle -> Delete -> Logout works completely without manual workarounds.
- [x] **Full-Stack Request Traceability:** Documented from React component fetch through Express routes, controllers, Mongoose schemas, and database response.
- [x] **Error & Empty Edge Cases:** Handled gracefully on both client and server; no raw stack traces or database errors exposed.
- [x] **Data Protection & Authorization:** Ownership checks enforce that users can only read, edit, or delete their own tasks.
- [x] **Secret Isolation:** Passwords hashed with bcrypt; `.env` files ignored by `.gitignore`; `.env.example` templates committed.
- [x] **Accessibility & Responsiveness:** Built mobile-first with semantic HTML, label associations, and keyboard navigation.

---

## 10. License & Acknowledgements
Developed as part of the **ICT Academy of Kerala (ICTAK)** Full Stack Application Development Internship Program.
Author: **Athulya**
