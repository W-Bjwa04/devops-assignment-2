# 📋 Assignment Submission Checklist

## Before You Start

### Required Accounts
- [ ] AWS Account (with EC2 access)
- [ ] Docker Hub Account
- [ ] GitHub Account

### Required Software (for testing locally)
- [ ] Git installed
- [ ] Docker installed
- [ ] Docker Compose installed
- [ ] Node.js 18+ (optional, for local development)

---

## Part I: Containerized Deployment

### Pre-Deployment
- [ ] Update `backend/.env` with production settings
- [ ] Update `frontend/.env` with API URL
- [ ] Test locally: `docker-compose up -d`
- [ ] Verify all containers running: `docker-compose ps`
- [ ] Test application at `http://localhost:3000`
- [ ] Stop local test: `docker-compose down`

### AWS Deployment
- [ ] Launch EC2 instance (Ubuntu 22.04, t2.medium)
- [ ] Configure Security Group (ports: 22, 80, 443, 3000, 5000, 8080)
- [ ] Save .pem key file securely
- [ ] Connect to EC2 via SSH
- [ ] Install Docker and Docker Compose
- [ ] Clone GitHub repository
- [ ] Build Docker images for backend and frontend
- [ ] Push images to Docker Hub
- [ ] Update docker-compose.yml to use Docker Hub images
- [ ] Run: `docker-compose up -d`
- [ ] Verify: `docker-compose ps`
- [ ] Test application at `http://EC2_PUBLIC_IP:3000`

### Docker Hub
- [ ] Login to Docker Hub
- [ ] Push backend image
- [ ] Push frontend image
- [ ] Verify images are public
- [ ] Note URLs for submission

### Screenshots for Part I
- [ ] EC2 instance running in AWS Console
- [ ] Security Group rules
- [ ] SSH terminal connected to EC2
- [ ] Docker version output
- [ ] Docker Compose version output
- [ ] `docker images` output
- [ ] `docker-compose ps` showing all containers running
- [ ] Docker Hub repositories showing images
- [ ] Application login page
- [ ] Application dashboard with sample todos
- [ ] Data persistence test (restart containers, data remains)

---

## Part II: Jenkins CI/CD Pipeline

### Jenkins Setup
- [ ] Install Java on EC2
- [ ] Install Jenkins on EC2
- [ ] Start Jenkins service
- [ ] Access Jenkins at `http://EC2_PUBLIC_IP:8080`
- [ ] Complete initial setup wizard
- [ ] Install required plugins:
  - [ ] Git Plugin
  - [ ] Pipeline Plugin
  - [ ] Docker Pipeline Plugin
  - [ ] GitHub Integration Plugin

### GitHub Configuration
- [ ] Create GitHub repository
- [ ] Push all code to GitHub
- [ ] Update Jenkinsfile with your GitHub URL
- [ ] Add instructor (qasimalik@gmail.com) as collaborator
- [ ] Configure webhook:
  - Payload URL: `http://EC2_PUBLIC_IP:8080/github-webhook/`
  - Content type: `application/json`
  - Trigger: Push events

### Jenkins Pipeline
- [ ] Create new Pipeline project in Jenkins
- [ ] Configure to use GitHub repository
- [ ] Set Script Path to `Jenkinsfile`
- [ ] Enable "GitHub hook trigger for GITScm polling"
- [ ] Save pipeline configuration
- [ ] Trigger first build manually
- [ ] Verify build succeeds (all stages green)
- [ ] Check application at `http://EC2_PUBLIC_IP:3001`

### Automated Trigger Test
- [ ] Make a small change to code
- [ ] Commit and push to GitHub
- [ ] Verify webhook triggers Jenkins build automatically
- [ ] Verify build succeeds
- [ ] Check application updated

### Final State
- [ ] Stop Jenkins deployment: `docker-compose -f docker-compose.jenkins.yml down`
- [ ] Keep Jenkins service running
- [ ] Keep Part I deployment UP for evaluation

### Screenshots for Part II
- [ ] Jenkins installation output
- [ ] Jenkins login page
- [ ] Jenkins dashboard
- [ ] Installed plugins list
- [ ] Pipeline configuration page
- [ ] GitHub webhook configuration
- [ ] GitHub webhook successful delivery
- [ ] First manual build success (all green)
- [ ] Console output of successful build
- [ ] `docker-compose -f docker-compose.jenkins.yml ps` output
- [ ] Application running on port 3001
- [ ] Application running on port 5001/health
- [ ] GitHub showing instructor as collaborator
- [ ] Automated trigger proof (commit → webhook → build)
- [ ] Build history showing automated trigger

---

## Documentation

### Files to Include in Report
- [ ] `backend/Dockerfile`
- [ ] `frontend/Dockerfile`
- [ ] `docker-compose.yml`
- [ ] `docker-compose.jenkins.yml`
- [ ] `Jenkinsfile`

### Report Structure
1. **Cover Page**
   - [ ] Assignment title
   - [ ] Student name and roll number
   - [ ] Course details
   - [ ] Submission date

2. **Table of Contents**
   - [ ] Page numbers
   - [ ] Section headings

3. **Introduction**
   - [ ] Project overview
   - [ ] Technologies used
   - [ ] Application features

4. **Part I: Containerized Deployment**
   - [ ] Step-by-step process with screenshots
   - [ ] Dockerfile explanations
   - [ ] docker-compose.yml explanation
   - [ ] Challenges and solutions
   - [ ] Results and testing

5. **Part II: Jenkins CI/CD Pipeline**
   - [ ] Jenkins setup process with screenshots
   - [ ] Pipeline configuration
   - [ ] docker-compose.jenkins.yml explanation
   - [ ] Jenkinsfile explanation
   - [ ] Webhook configuration
   - [ ] Testing and verification
   - [ ] Challenges and solutions

6. **Conclusion**
   - [ ] Summary of work done
   - [ ] Learning outcomes
   - [ ] Future improvements

7. **Appendices**
   - [ ] Complete code files
   - [ ] Configuration files
   - [ ] Additional screenshots

### Report Formatting
- [ ] Use professional template
- [ ] Clear headings and subheadings
- [ ] High-quality screenshots with captions
- [ ] Code blocks properly formatted
- [ ] Page numbers
- [ ] Consistent font and spacing
- [ ] Export as PDF

---

## Google Form Submission

### Information to Submit
- [ ] GitHub Repository URL
- [ ] Docker Hub Backend Image URL
- [ ] Docker Hub Frontend Image URL
- [ ] AWS EC2 Public IP Address
- [ ] Part I Frontend URL: `http://EC2_IP:3000`
- [ ] Part I Backend URL: `http://EC2_IP:5000`
- [ ] Part II Frontend URL: `http://EC2_IP:3001`
- [ ] Part II Backend URL: `http://EC2_IP:5001`
- [ ] Jenkins URL: `http://EC2_IP:8080`

### Submission Form
- [ ] Fill out: https://forms.gle/ubA9DRzQSudr2qhY6
- [ ] Double-check all URLs work
- [ ] Submit form
- [ ] Verify submission in response sheet

---

## Final Verification

### Part I Checks
- [ ] Application accessible at `http://EC2_IP:3000`
- [ ] Can register new account
- [ ] Can login
- [ ] Can create todos
- [ ] Can mark todos complete
- [ ] Can delete todos
- [ ] Data persists after container restart
- [ ] Docker images on Docker Hub are public

### Part II Checks
- [ ] Jenkins accessible at `http://EC2_IP:8080`
- [ ] Instructor added as GitHub collaborator
- [ ] Webhook configured and working
- [ ] Pipeline builds successfully
- [ ] **Jenkins containers are DOWN** (as required)
- [ ] Part I containers still UP

### Security Checks
- [ ] EC2 security group properly configured
- [ ] All required ports open
- [ ] SSH access works
- [ ] No sensitive data in GitHub repo
- [ ] .env files not committed (use .gitignore)

### Documentation Checks
- [ ] README.md complete
- [ ] AWS_DEPLOYMENT_GUIDE.md complete
- [ ] All code files present
- [ ] .gitignore configured
- [ ] Report PDF generated
- [ ] All screenshots captured

---

## Important Reminders

### Do NOT Forget
1. ✅ Add instructor as collaborator on GitHub
2. ✅ Make Docker Hub images PUBLIC
3. ✅ Keep Part I containers RUNNING
4. ✅ Keep Part II containers DOWN (but Jenkins service running)
5. ✅ Test webhook trigger before submission
6. ✅ Take screenshots as you go
7. ✅ Include MONGODB volume in docker-compose
8. ✅ Update Jenkinsfile with YOUR GitHub URL
9. ✅ Use different ports for Part II (3001, 5001)
10. ✅ Use different container names for Part II

### URLs to Replace
Before submission, replace these placeholders:
- `YOUR_USERNAME` → Your GitHub username
- `YOUR_DOCKERHUB_USERNAME` → Your Docker Hub username
- `EC2_PUBLIC_IP` → Your actual EC2 public IP
- `YOUR_EC2_PUBLIC_IP` → Your actual EC2 public IP

### Cost Management
- [ ] Stop EC2 instance when not actively testing
- [ ] Terminate instance after evaluation (if not needed)
- [ ] Monitor AWS billing dashboard
- [ ] Consider using AWS Free Tier eligible options

---

## Submission Timeline

### Recommended Schedule
1. **Day 1-2**: Set up development environment, test locally
2. **Day 3-4**: Deploy Part I on AWS, take screenshots
3. **Day 5-6**: Set up Jenkins, configure Part II, take screenshots
4. **Day 7**: Test everything, prepare report
5. **Day 8**: Final review, submit

### Before Final Submission
- [ ] Test all URLs one more time
- [ ] Review report for typos
- [ ] Ensure all screenshots are clear
- [ ] Verify GitHub repository is accessible
- [ ] Check Docker Hub images are public
- [ ] Confirm instructor is collaborator
- [ ] Test webhook one last time
- [ ] Make backup of everything

---

## Support Resources

### Documentation
- README.md - Project overview
- AWS_DEPLOYMENT_GUIDE.md - Detailed deployment steps
- Docker documentation: https://docs.docker.com
- Jenkins documentation: https://www.jenkins.io/doc
- AWS EC2 documentation: https://docs.aws.amazon.com/ec2

### Troubleshooting
- Check AWS_DEPLOYMENT_GUIDE.md "Troubleshooting" section
- Review container logs: `docker-compose logs -f`
- Check Jenkins console output
- Verify security group rules
- Test network connectivity

---

## Contact for Questions

**Instructor**: Qasim Malik  
**Email**: qasimalik@gmail.com

**Course**: CSC483 – DevOps  
**Semester**: Fall 2025  
**Institution**: COMSATS University, Islamabad

---

## Good Luck! 🚀

Remember:
- Take your time with each step
- Document everything
- Test thoroughly
- Ask questions if stuck
- Submit before deadline

**You've got this!** 💪
