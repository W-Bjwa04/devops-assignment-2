# AWS EC2 Deployment Guide - DevOps Assignment 2

This guide provides step-by-step instructions for deploying the MERN Todo Task Manager application on AWS EC2 for both Part I and Part II of the assignment.

## Table of Contents
- [Prerequisites](#prerequisites)
- [Part I: Containerized Deployment](#part-i-containerized-deployment)
- [Part II: Jenkins CI/CD Pipeline](#part-ii-jenkins-cicd-pipeline)

---

## Prerequisites

### AWS Account Setup
1. Create an AWS account if you don't have one
2. Access the AWS Management Console
3. Navigate to EC2 Dashboard

### Local Setup
1. Install Git on your local machine
2. Create a GitHub account and repository
3. Push your code to GitHub

---

## Part I: Containerized Deployment

### Step 1: Launch EC2 Instance

1. **Go to EC2 Dashboard** → Click "Launch Instance"

2. **Configure Instance:**
   - **Name**: `todo-app-server`
   - **AMI**: Ubuntu Server 22.04 LTS (Free tier eligible)
   - **Instance Type**: t2.medium (minimum recommended) or t2.large
   - **Key Pair**: Create new key pair
     - Name: `todo-app-key`
     - Type: RSA
     - Format: .pem (for SSH)
     - Download and save securely

3. **Network Settings:**
   - Click "Edit"
   - **Auto-assign public IP**: Enable
   - **Firewall (Security Groups)**: Create new security group
     - Name: `todo-app-security-group`
   - **Inbound Rules**: Add the following rules:
     ```
     Type              Protocol    Port Range    Source
     SSH               TCP         22            0.0.0.0/0
     HTTP              TCP         80            0.0.0.0/0
     HTTPS             TCP         443           0.0.0.0/0
     Custom TCP        TCP         3000          0.0.0.0/0  (Frontend)
     Custom TCP        TCP         5000          0.0.0.0/0  (Backend)
     Custom TCP        TCP         8080          0.0.0.0/0  (Jenkins)
     ```

4. **Storage**: 
   - Configure: 20 GB gp3 (minimum)

5. **Launch Instance** → Wait until instance state is "Running"

### Step 2: Connect to EC2 Instance

1. **Get Instance Details:**
   - Select your instance
   - Copy the "Public IPv4 DNS" or "Public IPv4 address"

2. **Connect via SSH (Windows PowerShell):**
   ```powershell
   # Navigate to where you saved the key file
   cd C:\Users\YourUsername\Downloads
   
   # Connect to EC2 (replace with your instance DNS)
   ssh -i "todo-app-key.pem" ubuntu@ec2-XX-XX-XX-XX.compute-1.amazonaws.com
   ```

   **Note for Windows**: If you get a permissions error, you may need to use WSL or Git Bash.

3. **If using Git Bash (recommended for Windows):**
   ```bash
   chmod 400 todo-app-key.pem
   ssh -i "todo-app-key.pem" ubuntu@ec2-XX-XX-XX-XX.compute-1.amazonaws.com
   ```

### Step 3: Install Docker and Docker Compose

1. **Update system packages:**
   ```bash
   sudo apt update
   sudo apt upgrade -y
   ```

2. **Install Docker:**
   ```bash
   # Install required packages
   sudo apt install -y apt-transport-https ca-certificates curl software-properties-common
   
   # Add Docker's official GPG key
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
   
   # Add Docker repository
   echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   
   # Update package database
   sudo apt update
   
   # Install Docker
   sudo apt install -y docker-ce docker-ce-cli containerd.io
   
   # Start and enable Docker
   sudo systemctl start docker
   sudo systemctl enable docker
   
   # Add current user to docker group
   sudo usermod -aG docker $USER
   
   # Apply group changes
   newgrp docker
   ```

3. **Verify Docker installation:**
   ```bash
   docker --version
   docker run hello-world
   ```

4. **Install Docker Compose:**
   ```bash
   # Download Docker Compose
   sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
   
   # Make it executable
   sudo chmod +x /usr/local/bin/docker-compose
   
   # Verify installation
   docker-compose --version
   ```

### Step 4: Clone Repository and Deploy Application

1. **Install Git:**
   ```bash
   sudo apt install -y git
   ```

2. **Clone your repository:**
   ```bash
   # Replace with your GitHub repository URL
   git clone https://github.com/YOUR_USERNAME/devops-assignment-2.git
   cd devops-assignment-2
   ```

3. **Build Docker images:**
   ```bash
   # Build backend image
   cd backend
   docker build -t your-dockerhub-username/todo-backend:latest .
   cd ..
   
   # Build frontend image
   cd frontend
   docker build -t your-dockerhub-username/todo-frontend:latest .
   cd ..
   ```

4. **Push images to Docker Hub:**
   ```bash
   # Login to Docker Hub
   docker login
   # Enter your Docker Hub username and password
   
   # Push images
   docker push your-dockerhub-username/todo-backend:latest
   docker push your-dockerhub-username/todo-frontend:latest
   ```

5. **Update docker-compose.yml** to use your Docker Hub images:
   ```bash
   nano docker-compose.yml
   ```
   
   Change the build contexts to use your images:
   ```yaml
   backend:
     image: your-dockerhub-username/todo-backend:latest
     # Remove or comment out: build: ./backend
   
   frontend:
     image: your-dockerhub-username/todo-frontend:latest
     # Remove or comment out: build: ./frontend
   ```

6. **Start the application:**
   ```bash
   docker-compose up -d
   ```

7. **Verify containers are running:**
   ```bash
   docker-compose ps
   docker-compose logs
   ```

### Step 5: Access Your Application

1. **Open your browser and navigate to:**
   - Frontend: `http://YOUR_EC2_PUBLIC_IP:3000`
   - Backend API: `http://YOUR_EC2_PUBLIC_IP:5000/health`

2. **Test the application:**
   - Register a new account
   - Login
   - Create some todos
   - Mark todos as complete
   - Delete todos

### Step 6: Screenshots for Report (Part I)

Take screenshots of:
1. EC2 instance running in AWS console
2. Security group inbound rules
3. SSH connection to EC2
4. Docker and Docker Compose versions
5. `docker-compose ps` output showing running containers
6. `docker images` showing your images
7. Docker Hub repository with your images
8. Application login page
9. Application dashboard with todos
10. MongoDB data persistence (restart containers and verify data persists)

---

## Part II: Jenkins CI/CD Pipeline

### Step 7: Launch Second EC2 Instance for Jenkins

**Option 1: Use the same EC2 instance** (for cost-saving, but ensure t2.medium or larger)

**Option 2: Launch a new EC2 instance** (recommended):
- Follow Step 1 again with name: `jenkins-server`
- Use same security group or create new one with port 8080 open

For this guide, we'll use the **same EC2 instance**.

### Step 8: Install Jenkins

1. **SSH into your EC2 instance** (if not already connected)

2. **Install Java (Jenkins requirement):**
   ```bash
   sudo apt update
   sudo apt install -y openjdk-11-jdk
   java -version
   ```

3. **Install Jenkins:**
   ```bash
   # Add Jenkins repository key
   curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null
   
   # Add Jenkins repository
   echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
   
   # Update and install Jenkins
   sudo apt update
   sudo apt install -y jenkins
   
   # Start Jenkins
   sudo systemctl start jenkins
   sudo systemctl enable jenkins
   
   # Check Jenkins status
   sudo systemctl status jenkins
   ```

4. **Configure Jenkins user for Docker:**
   ```bash
   sudo usermod -aG docker jenkins
   sudo systemctl restart jenkins
   ```

### Step 9: Access and Configure Jenkins

1. **Get Jenkins initial admin password:**
   ```bash
   sudo cat /var/lib/jenkins/secrets/initialAdminPassword
   ```
   Copy this password.

2. **Access Jenkins Web Interface:**
   - Open browser: `http://YOUR_EC2_PUBLIC_IP:8080`

3. **Initial Setup:**
   - Paste the initial admin password
   - Click "Install suggested plugins"
   - Wait for plugins to install

4. **Create First Admin User:**
   - Username: `admin`
   - Password: (create a strong password)
   - Full name: Your Name
   - Email: Your Email
   - Click "Save and Continue"

5. **Jenkins URL:**
   - Keep default or change if needed
   - Click "Save and Finish"
   - Click "Start using Jenkins"

### Step 10: Install Required Jenkins Plugins

1. **Go to**: Dashboard → Manage Jenkins → Manage Plugins

2. **Install the following plugins:**
   - Navigate to "Available" tab and search for:
     - ✓ Git plugin (usually already installed)
     - ✓ Pipeline plugin (usually already installed)
     - ✓ Docker Pipeline plugin
     - ✓ GitHub Integration Plugin
   
3. **Install without restart** or check "Restart Jenkins when installation is complete"

### Step 11: Configure Jenkins Credentials

1. **Add GitHub Credentials** (if private repo):
   - Dashboard → Manage Jenkins → Manage Credentials
   - Click "(global)" → "Add Credentials"
   - Kind: "Username with password"
   - Username: Your GitHub username
   - Password: GitHub Personal Access Token
     - Go to GitHub: Settings → Developer settings → Personal access tokens → Generate new token
     - Select scopes: `repo`, `admin:repo_hook`
   - ID: `github-credentials`
   - Click "Create"

### Step 12: Prepare Application for Jenkins

1. **Stop Part I containers:**
   ```bash
   cd ~/devops-assignment-2
   docker-compose down
   ```

2. **Ensure docker-compose.jenkins.yml is in your repository**

3. **Update Jenkinsfile with your GitHub URL:**
   ```bash
   nano Jenkinsfile
   ```
   Change line:
   ```
   GIT_REPO = 'https://github.com/YOUR_USERNAME/devops-assignment-2.git'
   ```

4. **Commit and push changes:**
   ```bash
   git add .
   git commit -m "Update Jenkinsfile with repository URL"
   git push origin main
   ```

5. **Add instructor as collaborator:**
   - Go to GitHub repository
   - Settings → Collaborators → Add people
   - Enter: qasimalik@gmail.com
   - Send invitation

### Step 13: Create Jenkins Pipeline

1. **Create New Pipeline:**
   - Jenkins Dashboard → New Item
   - Name: `Todo-App-Build-Pipeline`
   - Type: "Pipeline"
   - Click "OK"

2. **Configure Pipeline:**
   
   **General Section:**
   - Description: "Automated build pipeline for Todo Task Manager"
   - ✓ Check "GitHub project"
   - Project URL: `https://github.com/YOUR_USERNAME/devops-assignment-2/`

   **Build Triggers:**
   - ✓ Check "GitHub hook trigger for GITScm polling"

   **Pipeline Section:**
   - Definition: "Pipeline script from SCM"
   - SCM: "Git"
   - Repository URL: `https://github.com/YOUR_USERNAME/devops-assignment-2.git`
   - Credentials: Select your GitHub credentials (if private repo)
   - Branch: `*/main`
   - Script Path: `Jenkinsfile`

3. **Save the pipeline**

### Step 14: Configure GitHub Webhook

1. **Go to your GitHub repository**

2. **Settings → Webhooks → Add webhook**

3. **Configure Webhook:**
   - Payload URL: `http://YOUR_EC2_PUBLIC_IP:8080/github-webhook/`
   - Content type: `application/json`
   - Which events: "Just the push event"
   - ✓ Active
   - Click "Add webhook"

4. **Verify webhook:**
   - You should see a green checkmark after GitHub pings Jenkins

### Step 15: Test the Pipeline

1. **Manual Trigger (First Test):**
   - Go to Jenkins Dashboard
   - Click on "Todo-App-Build-Pipeline"
   - Click "Build Now"
   - Watch the build progress in "Build History"
   - Click on the build number → "Console Output" to see logs

2. **Verify Deployment:**
   ```bash
   docker-compose -f docker-compose.jenkins.yml ps
   ```
   
3. **Access the application:**
   - Frontend: `http://YOUR_EC2_PUBLIC_IP:3001`
   - Backend: `http://YOUR_EC2_PUBLIC_IP:5001/health`

### Step 16: Test Automated Pipeline (GitHub Push Trigger)

1. **Make a small change to your code:**
   ```bash
   cd ~/devops-assignment-2
   
   # Make a small change (e.g., update README)
   echo "\n## Test commit for Jenkins trigger" >> README.md
   
   # Commit and push
   git add .
   git commit -m "Test Jenkins webhook trigger"
   git push origin main
   ```

2. **Watch Jenkins:**
   - The pipeline should automatically trigger
   - Monitor the build progress in Jenkins dashboard

3. **Verify the deployment updated**

### Step 17: Bring Down Jenkins Environment (For Evaluation)

**IMPORTANT**: As per assignment requirements, keep the Jenkins environment down initially.

```bash
cd ~/devops-assignment-2
docker-compose -f docker-compose.jenkins.yml down
```

The instructor will trigger the pipeline via GitHub push to bring it up.

### Step 18: Screenshots for Report (Part II)

Take screenshots of:
1. Jenkins login page
2. Jenkins dashboard with installed plugins
3. Pipeline configuration page
4. GitHub webhook configuration
5. Successful pipeline build (all stages green)
6. Console output of successful build
7. `docker-compose -f docker-compose.jenkins.yml ps` output
8. Application running on ports 3001 and 5001
9. GitHub repository with instructor added as collaborator
10. Proof of automated trigger (commit → webhook → Jenkins build)

---

## Troubleshooting

### Common Issues and Solutions

**1. Cannot connect to EC2 instance:**
- Check security group allows SSH (port 22)
- Verify using correct key file
- Ensure instance is running

**2. Docker permission denied:**
```bash
sudo usermod -aG docker $USER
newgrp docker
```

**3. Containers not starting:**
```bash
docker-compose logs
docker ps -a
```

**4. Port already in use:**
```bash
sudo netstat -tulpn | grep :PORT_NUMBER
sudo kill -9 PID
```

**5. Jenkins cannot access Docker:**
```bash
sudo usermod -aG docker jenkins
sudo systemctl restart jenkins
```

**6. MongoDB connection issues:**
- Ensure MongoDB container is running
- Check docker network connectivity
- Verify MONGODB_URI in environment variables

**7. GitHub webhook not triggering:**
- Verify webhook URL is correct
- Check Jenkins GitHub plugin is installed
- Ensure EC2 security group allows port 8080
- Check webhook delivery history in GitHub

---

## Cleanup (After Evaluation)

To stop and remove all containers:

```bash
# Part I
docker-compose down -v

# Part II
docker-compose -f docker-compose.jenkins.yml down -v

# Stop Jenkins
sudo systemctl stop jenkins

# Optional: Remove all Docker images
docker system prune -a
```

To terminate EC2 instance:
1. Go to EC2 Dashboard
2. Select your instance
3. Instance State → Terminate Instance

---

## Important Notes for Submission

1. **Docker Hub URLs:**
   - Backend: `https://hub.docker.com/r/YOUR_USERNAME/todo-backend`
   - Frontend: `https://hub.docker.com/r/YOUR_USERNAME/todo-frontend`

2. **GitHub Repository:**
   - URL: `https://github.com/YOUR_USERNAME/devops-assignment-2`
   - Ensure instructor (qasimalik@gmail.com) is added as collaborator

3. **Application URLs:**
   - Part I Frontend: `http://YOUR_EC2_IP:3000`
   - Part I Backend: `http://YOUR_EC2_IP:5000`
   - Part II Frontend: `http://YOUR_EC2_IP:3001`
   - Part II Backend: `http://YOUR_EC2_IP:5001`
   - Jenkins: `http://YOUR_EC2_IP:8080`

4. **Google Form Submission:**
   - Fill out: https://forms.gle/ubA9DRzQSudr2qhY6
   - Include all URLs mentioned above

5. **Report Requirements:**
   - Well-formatted document (PDF recommended)
   - Include all micro steps with screenshots
   - Attach configuration files:
     - `backend/Dockerfile`
     - `frontend/Dockerfile`
     - `docker-compose.yml`
     - `docker-compose.jenkins.yml`
     - `Jenkinsfile`
   - Document challenges faced and solutions

---

## Tips for Success

1. **Take screenshots as you go** - Don't wait until the end
2. **Test thoroughly** - Ensure everything works before submitting
3. **Keep Jenkins environment down** until instructor tests
4. **Document everything** - More detail is better
5. **Backup your key files** - Store .pem file safely
6. **Monitor costs** - Stop/terminate instances when not in use
7. **Test the webhook** - Make sure automated triggers work
8. **Clean commit history** - Professional commits look better

---

## Support

If you encounter issues:
1. Check AWS CloudWatch logs
2. Review Docker container logs: `docker-compose logs -f`
3. Check Jenkins console output
4. Verify security group rules
5. Ensure all ports are accessible

Good luck with your assignment! 🚀
