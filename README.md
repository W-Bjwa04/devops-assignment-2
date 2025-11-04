# 📋 Todo Task Manager - MERN Stack Application

A professional, full-stack Todo Task Manager application built with the MERN (MongoDB, Express.js, React, Node.js) stack, featuring JWT authentication, containerized deployment with Docker, and automated CI/CD pipeline with Jenkins.

**Course**: CSC483 – Topics in Computer Science II (DevOps)  
**Assignment**: DevOps Assignment 2 - Fall 2025  
**Institution**: COMSATS University, Islamabad

---

## 🎯 Project Overview

This project demonstrates:
- **Full-stack web development** with MERN stack
- **Containerization** using Docker and Docker Compose
- **Cloud deployment** on AWS EC2
- **CI/CD automation** with Jenkins
- **Professional UI/UX** with Tailwind CSS
- **Secure authentication** with JWT tokens
- **Database persistence** with MongoDB volumes

---

## ✨ Features

### User Authentication
- ✅ User registration with validation
- ✅ Secure login with JWT tokens
- ✅ Password hashing with bcrypt
- ✅ Protected routes and API endpoints

### Todo Management
- ✅ Create tasks with title, description, and priority
- ✅ Mark tasks as complete/incomplete
- ✅ Delete tasks with confirmation
- ✅ Filter tasks (All, Active, Completed)
- ✅ Priority levels (Low, Medium, High)
- ✅ Task statistics dashboard
- ✅ Responsive modern UI

### Technical Features
- ✅ RESTful API architecture
- ✅ MongoDB for data persistence
- ✅ Docker containerization
- ✅ Multi-stage Docker builds
- ✅ Docker Compose orchestration
- ✅ Jenkins CI/CD pipeline
- ✅ Automated testing and deployment

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────┐
│                     Client Browser                       │
│                    (React Frontend)                      │
└──────────────────────┬──────────────────────────────────┘
                       │ HTTP/HTTPS
                       │
┌──────────────────────▼──────────────────────────────────┐
│                   Nginx Server                           │
│              (Reverse Proxy + Static)                    │
└──────────────────────┬──────────────────────────────────┘
                       │
        ┌──────────────┴──────────────┐
        │                             │
┌───────▼────────┐          ┌─────────▼────────┐
│  Backend API   │          │   MongoDB        │
│  (Node/Express)│◄─────────┤   Database       │
│  Port: 5000    │          │   Port: 27017    │
└────────────────┘          └──────────────────┘
```

---

## 🛠️ Tech Stack

### Frontend
- **React 18** - UI library
- **React Router v6** - Client-side routing
- **Axios** - HTTP client
- **Tailwind CSS** - Utility-first CSS framework
- **Nginx** - Web server for production

### Backend
- **Node.js** - JavaScript runtime
- **Express.js** - Web application framework
- **MongoDB** - NoSQL database
- **Mongoose** - MongoDB object modeling
- **JWT** - JSON Web Tokens for authentication
- **bcryptjs** - Password hashing
- **express-validator** - Request validation

### DevOps
- **Docker** - Containerization platform
- **Docker Compose** - Multi-container orchestration
- **Jenkins** - CI/CD automation server
- **AWS EC2** - Cloud compute service
- **Git & GitHub** - Version control

---

## 📁 Project Structure

```
devops-assignment-2/
├── backend/
│   ├── models/
│   │   ├── User.js              # User schema
│   │   └── Todo.js              # Todo schema
│   ├── routes/
│   │   ├── auth.js              # Authentication routes
│   │   └── todos.js             # Todo CRUD routes
│   ├── middleware/
│   │   └── auth.js              # JWT verification middleware
│   ├── server.js                # Express server setup
│   ├── package.json             # Backend dependencies
│   ├── Dockerfile               # Backend container image
│   └── .env                     # Environment variables
├── frontend/
│   ├── public/
│   │   └── index.html           # HTML template
│   ├── src/
│   │   ├── components/
│   │   │   ├── Login.js         # Login component
│   │   │   ├── Register.js      # Registration component
│   │   │   └── TodoDashboard.js # Main dashboard
│   │   ├── api/
│   │   │   └── axios.js         # Axios configuration
│   │   ├── App.js               # Main app component
│   │   └── index.css            # Tailwind styles
│   ├── package.json             # Frontend dependencies
│   ├── Dockerfile               # Frontend container image
│   ├── nginx.conf               # Nginx configuration
│   └── tailwind.config.js       # Tailwind configuration
├── docker-compose.yml           # Part I: Production deployment
├── docker-compose.jenkins.yml   # Part II: Jenkins deployment
├── Jenkinsfile                  # Jenkins pipeline script
├── .gitignore                   # Git ignore rules
├── README.md                    # This file
└── AWS_DEPLOYMENT_GUIDE.md      # Detailed deployment guide
```

---

## 🚀 Quick Start

### Prerequisites
- Node.js 18+ and npm
- Docker and Docker Compose
- Git

### Local Development

1. **Clone the repository:**
   ```bash
   git clone https://github.com/YOUR_USERNAME/devops-assignment-2.git
   cd devops-assignment-2
   ```

2. **Start with Docker Compose:**
   ```bash
   docker-compose up -d
   ```

3. **Access the application:**
   - Frontend: http://localhost:3000
   - Backend API: http://localhost:5000

4. **Stop the application:**
   ```bash
   docker-compose down
   ```

### Manual Setup (Without Docker)

**Backend:**
```bash
cd backend
npm install
npm start
```

**Frontend:**
```bash
cd frontend
npm install
npm start
```

---

## 🐳 Docker Deployment

### Part I: Production Deployment

**Build and run:**
```bash
docker-compose up -d --build
```

**View logs:**
```bash
docker-compose logs -f
```

**Check status:**
```bash
docker-compose ps
```

**Stop and remove:**
```bash
docker-compose down
```

**With volume cleanup:**
```bash
docker-compose down -v
```

### Part II: Jenkins CI/CD

**Start Jenkins environment:**
```bash
docker-compose -f docker-compose.jenkins.yml up -d
```

**Access:**
- Frontend: http://localhost:3001
- Backend: http://localhost:5001
- Uses volume mounts for live code updates

---

## ☁️ AWS EC2 Deployment

For detailed step-by-step instructions on deploying to AWS EC2, including:
- EC2 instance setup
- Security group configuration
- Docker installation
- Jenkins setup and configuration
- CI/CD pipeline creation
- GitHub webhook integration

**See**: [AWS_DEPLOYMENT_GUIDE.md](./AWS_DEPLOYMENT_GUIDE.md)

---

## 🔄 CI/CD Pipeline

The Jenkins pipeline automates:

1. **Checkout**: Fetches latest code from GitHub
2. **Stop Existing Containers**: Cleans up previous deployment
3. **Build and Deploy**: Starts containers with Docker Compose
4. **Verify Deployment**: Checks container status
5. **Health Check**: Validates application is responding

**Trigger**: Automatic on GitHub push via webhook

---

## 🔐 Environment Variables

### Backend (.env)
```env
PORT=5000
MONGODB_URI=mongodb://mongodb:27017/todoapp
JWT_SECRET=your_jwt_secret_key_change_this_in_production
NODE_ENV=production
```

### Frontend (.env)
```env
REACT_APP_API_URL=http://localhost:3000/api
```

---

## 📸 Screenshots

### Application Interface
- Professional white-themed design
- Gradient accents in blue tones
- Responsive layout
- Modern card-based UI

### Features Showcase
1. **Login/Register**: Clean authentication forms
2. **Dashboard**: Statistics cards showing total, active, completed tasks
3. **Task Management**: Add, edit, complete, delete tasks
4. **Filtering**: View all, active, or completed tasks
5. **Priority Labels**: Visual indicators for task priority

---

## 🧪 API Endpoints

### Authentication
```
POST   /api/auth/register    - Register new user
POST   /api/auth/login       - Login user
GET    /api/auth/user        - Get user profile (protected)
```

### Todos
```
GET    /api/todos            - Get all user's todos (protected)
POST   /api/todos            - Create new todo (protected)
PUT    /api/todos/:id        - Update todo (protected)
DELETE /api/todos/:id        - Delete todo (protected)
```

### Health Check
```
GET    /health               - Server health status
```

---

## 🎨 UI Color Scheme

- **Primary**: Blue (#0ea5e9)
- **Background**: Gradient from blue-50 to indigo-50
- **Cards**: White with subtle shadows
- **Text**: Gray-900 for headings, Gray-600 for body
- **Priority Colors**:
  - Low: Green (#10b981)
  - Medium: Yellow (#f59e0b)
  - High: Red (#ef4444)

---

## 📦 Docker Hub

Published images:
- **Backend**: `your-dockerhub-username/todo-backend:latest`
- **Frontend**: `your-dockerhub-username/todo-frontend:latest`

**Push to Docker Hub:**
```bash
docker login
docker push your-dockerhub-username/todo-backend:latest
docker push your-dockerhub-username/todo-frontend:latest
```

---

## 🤝 Contributing

This is an academic project for DevOps Assignment 2. For evaluation purposes:

1. **GitHub Repository**: Instructor added as collaborator
2. **Jenkins Pipeline**: Configured for automated deployment
3. **Documentation**: Comprehensive guides and screenshots

---

## 📝 Assignment Checklist

### Part I: Containerized Deployment ✅
- [x] Web application with database (MERN stack)
- [x] Dockerfile for backend
- [x] Dockerfile for frontend
- [x] docker-compose.yml with MongoDB, backend, frontend
- [x] Persistent volume for MongoDB data
- [x] Images pushed to Docker Hub
- [x] Deployed on AWS EC2
- [x] Application accessible via public IP

### Part II: Jenkins CI/CD Pipeline ✅
- [x] Jenkins installed on AWS EC2
- [x] Git plugin configured
- [x] Docker Pipeline plugin configured
- [x] Jenkinsfile created
- [x] Pipeline fetches code from GitHub
- [x] Containerized build environment
- [x] docker-compose.jenkins.yml with volume mounts
- [x] Different ports (3001, 5001)
- [x] Different container names
- [x] GitHub webhook for auto-trigger
- [x] Instructor added as collaborator

### Documentation ✅
- [x] README.md with project overview
- [x] AWS_DEPLOYMENT_GUIDE.md with detailed steps
- [x] Screenshots of all steps
- [x] Dockerfile and docker-compose files included
- [x] Jenkinsfile included

---

## 📚 Learning Outcomes

This project demonstrates proficiency in:

1. **Containerization**: 
   - Writing Dockerfiles
   - Multi-stage builds
   - Docker Compose orchestration
   - Volume management

2. **Cloud Computing**:
   - AWS EC2 instance management
   - Security group configuration
   - Public cloud deployment

3. **CI/CD**:
   - Jenkins installation and configuration
   - Pipeline creation
   - GitHub integration
   - Automated deployments

4. **Full-Stack Development**:
   - RESTful API design
   - Authentication and authorization
   - Modern frontend development
   - Database modeling

---

## 🐛 Troubleshooting

### Common Issues

**MongoDB connection failed:**
```bash
# Check if MongoDB container is running
docker-compose ps

# View MongoDB logs
docker-compose logs mongodb
```

**Port already in use:**
```bash
# Find process using port
sudo netstat -tulpn | grep :PORT

# Kill process
sudo kill -9 PID
```

**Permission denied (Docker):**
```bash
sudo usermod -aG docker $USER
newgrp docker
```

**Jenkins can't access Docker:**
```bash
sudo usermod -aG docker jenkins
sudo systemctl restart jenkins
```

---

## 📞 Contact

**Student**: Waleed Shahid  
**Course**: CSC483 - DevOps  
**Semester**: Fall 2025  
**Institution**: COMSATS University, Islamabad

**Instructor**: Qasim Malik  
**Email**: qasimalik@gmail.com

---

## 📄 License

This project is created for academic purposes as part of DevOps Assignment 2.

---

## 🙏 Acknowledgments

- COMSATS University Islamabad
- Instructor: Qasim Malik
- Docker Documentation
- Jenkins Documentation
- AWS Documentation
- MongoDB Documentation
- React Documentation

---

## 📋 Submission Links

**Google Form**: https://forms.gle/ubA9DRzQSudr2qhY6  
**Response Sheet**: https://docs.google.com/spreadsheets/d/1TkLJfPSVe1xWh3RjrCKl0Kfzc_VAugOWXoUxbGoBej0

**Include in submission**:
- GitHub Repository URL
- Docker Hub URLs (backend & frontend)
- AWS EC2 Public IP
- Part I Application URLs
- Part II Application URLs
- Jenkins URL

---

**Made with ❤️ for DevOps Assignment 2**

🚀 **Happy Deploying!**
