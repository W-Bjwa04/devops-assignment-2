# 📁 Complete Project Structure

```
devops-assignment-2/
│
├── 📂 backend/                          # Backend Node.js/Express API
│   ├── 📂 models/
│   │   ├── User.js                      # User schema with authentication fields
│   │   └── Todo.js                      # Todo schema with priority & completion
│   ├── 📂 routes/
│   │   ├── auth.js                      # POST /register, /login, GET /user
│   │   └── todos.js                     # GET, POST, PUT, DELETE /todos
│   ├── 📂 middleware/
│   │   └── auth.js                      # JWT verification middleware
│   ├── server.js                        # Express server setup & MongoDB connection
│   ├── package.json                     # Dependencies: express, mongoose, jwt, bcrypt
│   ├── .env                             # Environment variables (PORT, MONGODB_URI, JWT_SECRET)
│   ├── Dockerfile                       # Backend container image (Node 18 Alpine)
│   └── .dockerignore                    # Exclude node_modules from image
│
├── 📂 frontend/                         # Frontend React Application
│   ├── 📂 public/
│   │   └── index.html                   # HTML template
│   ├── 📂 src/
│   │   ├── 📂 components/
│   │   │   ├── Login.js                 # Login page with JWT authentication
│   │   │   ├── Register.js              # Registration page with validation
│   │   │   └── TodoDashboard.js         # Main dashboard with todo CRUD
│   │   ├── 📂 api/
│   │   │   └── axios.js                 # Axios instance with JWT interceptor
│   │   ├── App.js                       # Main app with routing & auth state
│   │   ├── index.js                     # React entry point
│   │   └── index.css                    # Tailwind CSS with custom styles
│   ├── package.json                     # Dependencies: react, react-router-dom, axios
│   ├── tailwind.config.js               # Tailwind theme configuration
│   ├── postcss.config.js                # PostCSS configuration for Tailwind
│   ├── nginx.conf                       # Nginx reverse proxy configuration
│   ├── Dockerfile                       # Multi-stage build (Node build → Nginx serve)
│   ├── .dockerignore                    # Exclude node_modules, build from image
│   └── .env                             # Frontend environment variables
│
├── 🐳 docker-compose.yml                # Part I: Production deployment
│   │   Services:
│   │   ├── mongodb (port 27017)         # MongoDB with persistent volume
│   │   ├── backend (port 5000)          # Built from ./backend/Dockerfile
│   │   └── frontend (port 3000)         # Built from ./frontend/Dockerfile
│
├── 🐳 docker-compose.jenkins.yml        # Part II: Jenkins CI/CD deployment
│   │   Services:
│   │   ├── mongodb (port 27018)         # MongoDB with separate volume
│   │   ├── backend (port 5001)          # Volume-mounted code from ./backend
│   │   └── frontend (port 3001)         # Volume-mounted code from ./frontend
│
├── 🔧 Jenkinsfile                       # Jenkins Pipeline Script
│   │   Stages:
│   │   ├── Checkout                     # Fetch code from GitHub
│   │   ├── Stop Existing Containers     # Clean up previous deployment
│   │   ├── Build and Deploy             # docker-compose up --build
│   │   ├── Verify Deployment            # Check container status
│   │   └── Health Check                 # Verify backend responds
│
├── 📄 .gitignore                        # Git ignore rules
│   │   Excludes: node_modules, .env, build, logs
│
├── 📘 README.md                         # Main project documentation
│   │   Sections:
│   │   ├── Project Overview             # Features, architecture, tech stack
│   │   ├── Quick Start                  # Local setup instructions
│   │   ├── Docker Deployment            # Part I & II instructions
│   │   ├── API Endpoints                # REST API documentation
│   │   ├── Assignment Checklist         # Requirements verification
│   │   └── Screenshots                  # UI showcase
│
├── 📖 AWS_DEPLOYMENT_GUIDE.md           # Comprehensive AWS deployment guide
│   │   Sections:
│   │   ├── Prerequisites                # AWS account, tools setup
│   │   ├── Part I Steps                 # EC2 setup, Docker install, deployment
│   │   ├── Part II Steps                # Jenkins setup, pipeline configuration
│   │   ├── Screenshots Guide            # What to capture for report
│   │   ├── Troubleshooting              # Common issues and solutions
│   │   └── Cleanup Instructions         # Post-evaluation cleanup
│
├── ✅ SUBMISSION_CHECKLIST.md           # Complete submission checklist
│   │   Sections:
│   │   ├── Before You Start             # Account setup, tools installation
│   │   ├── Part I Checklist             # Deployment steps with checkboxes
│   │   ├── Part II Checklist            # Jenkins setup with checkboxes
│   │   ├── Screenshots List             # Required screenshots for report
│   │   ├── Documentation Requirements   # Report structure and content
│   │   ├── Google Form Submission       # URLs to include
│   │   └── Final Verification           # Testing checklist
│
├── ⚡ QUICK_START.md                    # Command reference guide
│   │   Sections:
│   │   ├── Docker Commands              # Start, stop, logs, cleanup
│   │   ├── AWS EC2 Commands             # SSH, installation scripts
│   │   ├── Git Commands                 # Clone, commit, push
│   │   ├── Testing Commands             # API testing with curl
│   │   ├── Troubleshooting Commands     # Debug and fix issues
│   │   └── Quick Reference              # Ports, URLs, container names
│
└── 🎯 PROJECT_SUMMARY.md                # This file - project overview
    │   Sections:
    │   ├── What Has Been Created        # Complete file listing
    │   ├── Application Features         # Feature summary
    │   ├── Technical Architecture       # System design
    │   ├── Requirements Met             # Assignment verification
    │   ├── Next Steps                   # What you need to do
    │   └── Learning Outcomes            # Skills demonstrated
```

---

## 🎨 Application Flow Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                         User Browser                             │
└────────────────────────┬────────────────────────────────────────┘
                         │
                    HTTP Request
                         │
                         ▼
┌─────────────────────────────────────────────────────────────────┐
│                    Nginx (Frontend Container)                    │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │  React App (SPA)                                         │   │
│  │  ├── Login/Register → JWT Token                         │   │
│  │  ├── TodoDashboard → CRUD Operations                    │   │
│  │  └── Axios → Add JWT to headers                         │   │
│  └──────────────────────────────────────────────────────────┘   │
└────────────────────────┬────────────────────────────────────────┘
                         │
                    /api/* requests
                         │
                         ▼
┌─────────────────────────────────────────────────────────────────┐
│               Express API (Backend Container)                    │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │  Middleware: JWT Verification                            │   │
│  │  ├── /api/auth/register → Create User                   │   │
│  │  ├── /api/auth/login → Verify & Issue JWT               │   │
│  │  ├── /api/todos (GET) → Fetch user's todos              │   │
│  │  ├── /api/todos (POST) → Create new todo                │   │
│  │  ├── /api/todos/:id (PUT) → Update todo                 │   │
│  │  └── /api/todos/:id (DELETE) → Delete todo              │   │
│  └──────────────────────────────────────────────────────────┘   │
└────────────────────────┬────────────────────────────────────────┘
                         │
                   Mongoose ODM
                         │
                         ▼
┌─────────────────────────────────────────────────────────────────┐
│                MongoDB (Database Container)                      │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │  Collections:                                            │   │
│  │  ├── users                                               │   │
│  │  │   ├── _id, name, email, password (hashed)           │   │
│  │  │   └── createdAt                                      │   │
│  │  └── todos                                               │   │
│  │      ├── _id, user (ref), title, description           │   │
│  │      ├── completed, priority                            │   │
│  │      └── createdAt, updatedAt                           │   │
│  └──────────────────────────────────────────────────────────┘   │
│  Volume: mongodb_data (Persistent Storage)                       │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🔄 CI/CD Pipeline Flow

```
┌─────────────────────────────────────────────────────────────────┐
│                        Developer                                 │
│                     Makes Code Change                            │
└────────────────────────┬────────────────────────────────────────┘
                         │
                    git push origin main
                         │
                         ▼
┌─────────────────────────────────────────────────────────────────┐
│                       GitHub Repository                          │
│              devops-assignment-2 (main branch)                   │
└────────────────────────┬────────────────────────────────────────┘
                         │
                   Webhook Trigger
                         │
                         ▼
┌─────────────────────────────────────────────────────────────────┐
│                    Jenkins (CI/CD Server)                        │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │  Pipeline: Todo-App-Build-Pipeline                       │   │
│  │                                                           │   │
│  │  Stage 1: Checkout                                       │   │
│  │  ├── Clone repository from GitHub                       │   │
│  │  └── Fetch latest code                                  │   │
│  │                                                           │   │
│  │  Stage 2: Stop Existing Containers                      │   │
│  │  ├── docker-compose -f docker-compose.jenkins.yml down  │   │
│  │  └── Clean up previous deployment                       │   │
│  │                                                           │   │
│  │  Stage 3: Build and Deploy                              │   │
│  │  ├── docker-compose -f docker-compose.jenkins.yml up    │   │
│  │  ├── Start MongoDB container                            │   │
│  │  ├── Start Backend container (volume-mounted code)      │   │
│  │  └── Start Frontend container (volume-mounted code)     │   │
│  │                                                           │   │
│  │  Stage 4: Verify Deployment                             │   │
│  │  ├── Check container status                             │   │
│  │  └── Ensure all services running                        │   │
│  │                                                           │   │
│  │  Stage 5: Health Check                                  │   │
│  │  ├── Wait 30 seconds                                    │   │
│  │  ├── curl http://localhost:5001/health                  │   │
│  │  └── Verify backend responds                            │   │
│  │                                                           │   │
│  │  Post Actions:                                           │   │
│  │  ├── Success: Log deployment URLs                       │   │
│  │  ├── Failure: Show container logs                       │   │
│  │  └── Always: Clean workspace                            │   │
│  └──────────────────────────────────────────────────────────┘   │
└────────────────────────┬────────────────────────────────────────┘
                         │
                   Deployment Success
                         │
                         ▼
┌─────────────────────────────────────────────────────────────────┐
│                   Application Running on EC2                     │
│  Frontend: http://EC2_IP:3001                                    │
│  Backend:  http://EC2_IP:5001                                    │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🌐 Network Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                      AWS EC2 Instance                            │
│                   (Ubuntu 22.04, t2.medium)                      │
│                                                                   │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │            Docker Network: todo-network                    │  │
│  │                    (Bridge Driver)                         │  │
│  │                                                             │  │
│  │  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐    │  │
│  │  │  MongoDB     │  │  Backend     │  │  Frontend    │    │  │
│  │  │  Container   │  │  Container   │  │  Container   │    │  │
│  │  ├──────────────┤  ├──────────────┤  ├──────────────┤    │  │
│  │  │ mongo:7.0    │  │ Node 18      │  │ Nginx        │    │  │
│  │  │ Port: 27017  │  │ Port: 5000   │  │ Port: 80     │    │  │
│  │  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘    │  │
│  │         │                 │                 │             │  │
│  │         └─────────────────┴─────────────────┘             │  │
│  │                   Internal DNS                            │  │
│  └───────────────────────────────────────────────────────────┘  │
│                                                                   │
│  Port Mapping to Host:                                           │
│  ├── 27017:27017 (MongoDB)                                       │
│  ├── 5000:5000   (Backend API)                                   │
│  └── 3000:80     (Frontend Web)                                  │
│                                                                   │
│  Volumes:                                                         │
│  └── mongodb_data:/data/db (Persistent Storage)                  │
│                                                                   │
└────────────────────────┬──────────────────────────────────────── │
                         │
                    Public IP
                         │
                         ▼
┌─────────────────────────────────────────────────────────────────┐
│                    Security Group Rules                          │
│  Inbound:                                                         │
│  ├── SSH        TCP  22    0.0.0.0/0                             │
│  ├── HTTP       TCP  80    0.0.0.0/0                             │
│  ├── HTTPS      TCP  443   0.0.0.0/0                             │
│  ├── Custom TCP TCP  3000  0.0.0.0/0  (Frontend Part I)          │
│  ├── Custom TCP TCP  5000  0.0.0.0/0  (Backend Part I)           │
│  ├── Custom TCP TCP  3001  0.0.0.0/0  (Frontend Part II)         │
│  ├── Custom TCP TCP  5001  0.0.0.0/0  (Backend Part II)          │
│  └── Custom TCP TCP  8080  0.0.0.0/0  (Jenkins)                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 📊 Data Flow: Creating a Todo

```
1. User clicks "Add Task" button
   ↓
2. Modal opens with form
   ↓
3. User fills: Title, Description, Priority
   ↓
4. User clicks "Add Task"
   ↓
5. React calls: api.post('/todos', todoData)
   ↓
6. Axios interceptor adds JWT token to headers
   ↓
7. Request sent to: POST http://backend:5000/api/todos
   ↓
8. Express receives request
   ↓
9. Auth middleware verifies JWT token
   ↓
10. Extract user ID from token
    ↓
11. Validate request body (title required)
    ↓
12. Create new Todo document:
    {
      user: userIdFromToken,
      title: "Task Title",
      description: "Task Description",
      priority: "medium",
      completed: false,
      createdAt: Date.now()
    }
    ↓
13. Save to MongoDB
    ↓
14. MongoDB returns saved document with _id
    ↓
15. Express sends response: { _id, title, description, ... }
    ↓
16. React receives response
    ↓
17. Add new todo to state array
    ↓
18. UI updates automatically
    ↓
19. Close modal
    ↓
20. New todo appears in list
```

---

## 🔐 Authentication Flow

```
┌─────────────────────────────────────────────────────────────────┐
│                    Registration Flow                             │
└─────────────────────────────────────────────────────────────────┘

User fills registration form (name, email, password)
   ↓
POST /api/auth/register
   ↓
Validate input (name, email format, password length)
   ↓
Check if user exists (email unique)
   ↓
Hash password with bcrypt (salt rounds: 10)
   ↓
Save user to MongoDB
   ↓
Generate JWT token (expires in 7 days)
   ↓
Return: { token, user: { id, name, email } }
   ↓
Frontend stores token in localStorage
   ↓
Redirect to dashboard

┌─────────────────────────────────────────────────────────────────┐
│                        Login Flow                                │
└─────────────────────────────────────────────────────────────────┘

User fills login form (email, password)
   ↓
POST /api/auth/login
   ↓
Validate input
   ↓
Find user by email
   ↓
Compare password with bcrypt
   ↓
Generate JWT token (expires in 7 days)
   ↓
Return: { token, user: { id, name, email } }
   ↓
Frontend stores token in localStorage
   ↓
Redirect to dashboard

┌─────────────────────────────────────────────────────────────────┐
│                   Protected Route Access                         │
└─────────────────────────────────────────────────────────────────┘

User makes request to protected endpoint
   ↓
Axios interceptor adds header: x-auth-token: <JWT>
   ↓
Auth middleware extracts token from header
   ↓
Verify token with JWT secret
   ↓
Extract user ID from token payload
   ↓
Attach user ID to request: req.user.id
   ↓
Continue to route handler
   ↓
Route handler uses req.user.id to fetch user's data
```

---

## 📦 File Sizes (Approximate)

```
Backend Container Image:      ~150 MB
Frontend Container Image:     ~25 MB (multi-stage optimized)
MongoDB Container Image:      ~700 MB
Total Docker Images:          ~875 MB

Source Code:
- Backend:                    ~50 KB
- Frontend:                   ~100 KB
- Configuration:              ~10 KB
- Documentation:              ~150 KB
Total Source:                 ~310 KB

node_modules (not in images):
- Backend:                    ~100 MB
- Frontend:                   ~300 MB
```

---

## ⚙️ Environment Variables Reference

### Backend (.env)
```env
PORT=5000                              # Express server port
MONGODB_URI=mongodb://mongodb:27017/todoapp  # MongoDB connection string
JWT_SECRET=your_secret_here            # Secret for JWT signing
NODE_ENV=production                    # Environment mode
```

### Frontend (.env)
```env
REACT_APP_API_URL=http://localhost:3000/api  # Backend API URL
```

### Docker Compose Environment (inline)
```yaml
# Backend
PORT: 5000
MONGODB_URI: mongodb://mongodb:27017/todoapp
JWT_SECRET: your_jwt_secret_key_change_this_in_production
NODE_ENV: production

# Frontend (passed during build)
REACT_APP_API_URL: http://localhost:3000/api
```

---

## 🎯 Key Differences: Part I vs Part II

| Aspect | Part I (Production) | Part II (Jenkins) |
|--------|-------------------|-------------------|
| **Purpose** | Production deployment | CI/CD automated build |
| **Compose File** | docker-compose.yml | docker-compose.jenkins.yml |
| **Frontend Port** | 3000 | 3001 |
| **Backend Port** | 5000 | 5001 |
| **MongoDB Port** | 27017 | 27018 |
| **Container Names** | todo-* | jenkins-todo-* |
| **Volume Type** | MongoDB data only | Code + MongoDB data |
| **Image Build** | Dockerfile (built) | Volume-mounted (live) |
| **Network** | todo-network | jenkins-todo-network |
| **Volumes** | mongodb_data | jenkins_mongodb_data + code |
| **Code Updates** | Rebuild required | Live updates |
| **Use Case** | End users | Development/Testing |

---

## 🚀 Deployment Comparison

```
┌────────────────────────────────────────────────────────────┐
│                    Local Development                        │
│  npm start (backend) + npm start (frontend)                │
│  Pros: Fast, live reload, easy debugging                   │
│  Cons: No containerization, environment differences        │
└────────────────────────────────────────────────────────────┘

┌────────────────────────────────────────────────────────────┐
│              Part I: Docker Compose (Production)            │
│  docker-compose up -d                                      │
│  Pros: Production-like, isolated, portable                 │
│  Cons: Rebuild required for code changes                   │
└────────────────────────────────────────────────────────────┘

┌────────────────────────────────────────────────────────────┐
│            Part II: Jenkins + Docker Compose               │
│  Jenkins pipeline triggered by GitHub push                 │
│  Pros: Automated, CI/CD, version controlled                │
│  Cons: More complex setup                                  │
└────────────────────────────────────────────────────────────┘
```

---

## 📚 File Purpose Quick Reference

| File | Purpose | When to Edit |
|------|---------|--------------|
| `backend/server.js` | Express setup | Add new routes/middleware |
| `backend/models/*.js` | Database schemas | Modify data structure |
| `backend/routes/*.js` | API endpoints | Add/modify APIs |
| `frontend/App.js` | Main React app | Add routes/global state |
| `frontend/components/*.js` | UI components | Modify UI/UX |
| `backend/Dockerfile` | Backend image | Change Node version/deps |
| `frontend/Dockerfile` | Frontend image | Change build process |
| `docker-compose.yml` | Part I setup | Change ports/volumes |
| `docker-compose.jenkins.yml` | Part II setup | Jenkins configuration |
| `Jenkinsfile` | CI/CD pipeline | Modify build steps |
| `.env` files | Configuration | Change URLs/secrets |
| `nginx.conf` | Web server | Modify routing/proxy |

---

## 🎓 Skills Demonstrated

✅ Full-Stack Development (MERN)
✅ RESTful API Design
✅ Authentication & Authorization (JWT)
✅ Database Design (MongoDB)
✅ Frontend Development (React)
✅ UI/UX Design (Tailwind CSS)
✅ Containerization (Docker)
✅ Multi-Stage Builds
✅ Container Orchestration (Docker Compose)
✅ Volume Management
✅ Network Configuration
✅ CI/CD Pipeline (Jenkins)
✅ Git & GitHub
✅ Webhook Integration
✅ Cloud Deployment (AWS EC2)
✅ Security Groups & Firewalls
✅ Linux Server Administration
✅ Nginx Configuration
✅ Technical Documentation
✅ DevOps Best Practices

---

**Project created for CSC483 DevOps Assignment 2**
**COMSATS University, Islamabad - Fall 2025**

🚀 **Ready to deploy!**
