# Quick Start Scripts

This document contains helpful commands for quickly starting and managing the application.

## Windows PowerShell Commands

### Part I - Production Deployment

**Start the application:**
```powershell
docker-compose up -d
```

**View logs:**
```powershell
docker-compose logs -f
```

**Check container status:**
```powershell
docker-compose ps
```

**Stop the application:**
```powershell
docker-compose down
```

**Stop and remove volumes (reset database):**
```powershell
docker-compose down -v
```

**Rebuild and start:**
```powershell
docker-compose up -d --build
```

### Part II - Jenkins Deployment

**Start Jenkins environment:**
```powershell
docker-compose -f docker-compose.jenkins.yml up -d
```

**View logs:**
```powershell
docker-compose -f docker-compose.jenkins.yml logs -f
```

**Check container status:**
```powershell
docker-compose -f docker-compose.jenkins.yml ps
```

**Stop Jenkins environment:**
```powershell
docker-compose -f docker-compose.jenkins.yml down
```

### Docker Commands

**View all containers:**
```powershell
docker ps -a
```

**View all images:**
```powershell
docker images
```

**Remove all stopped containers:**
```powershell
docker container prune -f
```

**Remove all unused images:**
```powershell
docker image prune -a -f
```

**View Docker disk usage:**
```powershell
docker system df
```

**Clean up everything (be careful!):**
```powershell
docker system prune -a --volumes -f
```

### Build and Push to Docker Hub

**Login to Docker Hub:**
```powershell
docker login
```

**Build backend image:**
```powershell
cd backend
docker build -t your-dockerhub-username/todo-backend:latest .
cd ..
```

**Build frontend image:**
```powershell
cd frontend
docker build -t your-dockerhub-username/todo-frontend:latest .
cd ..
```

**Push images:**
```powershell
docker push your-dockerhub-username/todo-backend:latest
docker push your-dockerhub-username/todo-frontend:latest
```

### Git Commands

**Initialize and push to GitHub:**
```powershell
git init
git add .
git commit -m "Initial commit: MERN Todo Task Manager"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/devops-assignment-2.git
git push -u origin main
```

**Update and push changes:**
```powershell
git add .
git commit -m "Your commit message"
git push origin main
```

**Check status:**
```powershell
git status
```

**View commit history:**
```powershell
git log --oneline
```

---

## Linux/Mac Terminal Commands

### Part I - Production Deployment

**Start the application:**
```bash
docker-compose up -d
```

**View logs:**
```bash
docker-compose logs -f
```

**Check container status:**
```bash
docker-compose ps
```

**Stop the application:**
```bash
docker-compose down
```

**Stop and remove volumes:**
```bash
docker-compose down -v
```

**Rebuild and start:**
```bash
docker-compose up -d --build
```

### Part II - Jenkins Deployment

**Start Jenkins environment:**
```bash
docker-compose -f docker-compose.jenkins.yml up -d
```

**View logs:**
```bash
docker-compose -f docker-compose.jenkins.yml logs -f
```

**Check container status:**
```bash
docker-compose -f docker-compose.jenkins.yml ps
```

**Stop Jenkins environment:**
```bash
docker-compose -f docker-compose.jenkins.yml down
```

---

## AWS EC2 SSH Connection

### Windows (using Git Bash or WSL)

```bash
# Set correct permissions on key file
chmod 400 todo-app-key.pem

# Connect to EC2
ssh -i "todo-app-key.pem" ubuntu@ec2-XX-XX-XX-XX.compute-1.amazonaws.com
```

### Windows (using PowerShell)

```powershell
# Connect directly (if using OpenSSH)
ssh -i "todo-app-key.pem" ubuntu@ec2-XX-XX-XX-XX.compute-1.amazonaws.com
```

### Copy files to EC2 (SCP)

```bash
# Copy file to EC2
scp -i "todo-app-key.pem" local-file.txt ubuntu@ec2-XX-XX-XX-XX.compute-1.amazonaws.com:~/

# Copy directory to EC2
scp -i "todo-app-key.pem" -r local-directory ubuntu@ec2-XX-XX-XX-XX.compute-1.amazonaws.com:~/
```

---

## AWS EC2 Commands (After SSH)

### Install Docker and Docker Compose

```bash
# Update system
sudo apt update && sudo apt upgrade -y

# Install Docker
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo usermod -aG docker $USER
newgrp docker

# Install Docker Compose
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose

# Verify installations
docker --version
docker-compose --version
```

### Clone and Deploy

```bash
# Install Git
sudo apt install -y git

# Clone repository
git clone https://github.com/YOUR_USERNAME/devops-assignment-2.git
cd devops-assignment-2

# Start application
docker-compose up -d

# Check status
docker-compose ps
docker-compose logs
```

### Install Jenkins

```bash
# Install Java
sudo apt update
sudo apt install -y openjdk-11-jdk

# Install Jenkins
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt update
sudo apt install -y jenkins

# Start Jenkins
sudo systemctl start jenkins
sudo systemctl enable jenkins

# Add Jenkins to Docker group
sudo usermod -aG docker jenkins
sudo systemctl restart jenkins

# Get initial admin password
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

### Useful EC2 Commands

**Check running processes:**
```bash
ps aux | grep docker
ps aux | grep jenkins
```

**Check port usage:**
```bash
sudo netstat -tulpn | grep :3000
sudo netstat -tulpn | grep :5000
sudo netstat -tulpn | grep :8080
```

**View system resources:**
```bash
free -h          # Memory usage
df -h            # Disk usage
top              # CPU and process monitor
htop             # Better process monitor (install: sudo apt install htop)
```

**Check Docker logs:**
```bash
docker logs <container-id>
docker logs --follow <container-id>
docker logs --tail 100 <container-id>
```

---

## Testing Commands

### Test Backend API

**Health check:**
```bash
curl http://localhost:5000/health
```

**Register user:**
```bash
curl -X POST http://localhost:5000/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{"name":"Test User","email":"test@example.com","password":"password123"}'
```

**Login:**
```bash
curl -X POST http://localhost:5000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"test@example.com","password":"password123"}'
```

### Test Frontend

**Check if frontend is accessible:**
```bash
curl http://localhost:3000
```

### MongoDB Commands

**Access MongoDB container:**
```bash
docker exec -it todo-mongodb mongosh
```

**MongoDB shell commands:**
```javascript
// Show databases
show dbs

// Use todoapp database
use todoapp

// Show collections
show collections

// View users
db.users.find()

// View todos
db.todos.find()

// Exit
exit
```

---

## Troubleshooting Commands

### Container Issues

**Restart a container:**
```bash
docker restart <container-name>
```

**Stop a container:**
```bash
docker stop <container-name>
```

**Remove a container:**
```bash
docker rm <container-name>
```

**View container logs:**
```bash
docker logs <container-name>
docker logs --tail 50 <container-name>
```

**Execute command in container:**
```bash
docker exec -it <container-name> sh
```

### Network Issues

**List Docker networks:**
```bash
docker network ls
```

**Inspect a network:**
```bash
docker network inspect <network-name>
```

**Test connectivity between containers:**
```bash
docker exec <container-name> ping <other-container-name>
```

### Port Issues

**Kill process on specific port (Linux):**
```bash
sudo lsof -ti:PORT | xargs kill -9
```

**Kill process on specific port (Windows PowerShell):**
```powershell
Get-Process -Id (Get-NetTCPConnection -LocalPort PORT).OwningProcess | Stop-Process -Force
```

---

## Jenkins Commands

**Restart Jenkins:**
```bash
sudo systemctl restart jenkins
```

**Check Jenkins status:**
```bash
sudo systemctl status jenkins
```

**View Jenkins logs:**
```bash
sudo journalctl -u jenkins -f
```

**Jenkins CLI (from EC2):**
```bash
# Download Jenkins CLI
wget http://localhost:8080/jnlpJars/jenkins-cli.jar

# Example: List jobs
java -jar jenkins-cli.jar -s http://localhost:8080/ list-jobs
```

---

## Quick Reference

### Default Ports

| Service          | Port  | URL                        |
|------------------|-------|----------------------------|
| Frontend (Part I)| 3000  | http://localhost:3000      |
| Backend (Part I) | 5000  | http://localhost:5000      |
| MongoDB          | 27017 | mongodb://localhost:27017  |
| Frontend (Part II)| 3001 | http://localhost:3001      |
| Backend (Part II)| 5001  | http://localhost:5001      |
| Jenkins          | 8080  | http://localhost:8080      |

### Container Names

**Part I:**
- `todo-mongodb`
- `todo-backend`
- `todo-frontend`

**Part II:**
- `jenkins-todo-mongodb`
- `jenkins-todo-backend`
- `jenkins-todo-frontend`

### Volume Names

**Part I:**
- `mongodb_data`

**Part II:**
- `jenkins_mongodb_data`

---

## Emergency Cleanup

If everything goes wrong and you need to start fresh:

```bash
# Stop all containers
docker stop $(docker ps -aq)

# Remove all containers
docker rm $(docker ps -aq)

# Remove all images
docker rmi $(docker images -q)

# Remove all volumes
docker volume rm $(docker volume ls -q)

# Remove all networks
docker network prune -f

# Clean up everything
docker system prune -a --volumes -f
```

**⚠️ WARNING**: This will delete ALL Docker data on your system!

---

## Additional Resources

- Docker Docs: https://docs.docker.com
- Docker Compose Docs: https://docs.docker.com/compose
- Jenkins Docs: https://www.jenkins.io/doc
- MongoDB Docs: https://docs.mongodb.com
- AWS EC2 Docs: https://docs.aws.amazon.com/ec2
- Express.js Docs: https://expressjs.com
- React Docs: https://react.dev
- Tailwind CSS Docs: https://tailwindcss.com

---

**Happy Coding! 🚀**
