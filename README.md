# DevOps Activity: Static Website Deployment

## Project Overview
A complete DevOps implementation featuring:
- Static website (HTML/CSS)
- Docker containerization
- Docker Compose multi-service setup
- Kubernetes deployment
- Jenkins CI/CD pipeline

## Team Members
- Muhammad Rafey Tanweer - BSSE23034
- Syed Hashim Abbas - BSSE23084
- Omer Anwar - BSSE23044

## Technologies Used
- Docker & Docker Compose
- Kubernetes
- Jenkins
- Nginx
- Git & GitHub

## Project Structure
devops-activity/
├── Dockerfile.jenkins
├── Jenkinsfile
├── README.md
├── app
│   ├── Dockerfile
│   ├── images
│   ├── index.html
│   └── style.css
├── docker-compose.yml
└── kubernetes
    ├── deployment.yaml
    └── service.yaml

## Local Development

### Prerequisites
- Docker Desktop with Kubernetes enabled
- WSL integration with docker terminal 
- Git

## CI/CD Pipeline

### Jenkins Setup
1. Build custom Jenkins image with Docker and kubectl
2. Run Jenkins container with proper volume mounts
3. Create pipeline job using provided Jenkinsfile

### Pipeline Stages
1. **Checkout** - Verify workspace files
2. **Build** - Create Docker image
3. **Test** - Validate container
4. **Deploy** - Update Kubernetes deployment
5. **Verify** - Confirm deployment success

## Troubleshooting

### Docker Issues

**Problem:** Port already in use
```bash
# Find process using port
sudo lsof -i :9090

# Stop conflicting container
docker stop <container-id>
```

**Problem:** Build fails
```bash
# Clear Docker cache
docker system prune -a

# Rebuild without cache
docker-compose build --no-cache
```

### Kubernetes Issues

**Problem:** Pods not starting
```bash
# Check pod logs
kubectl logs <pod-name>

# Describe pod for events
kubectl describe pod <pod-name>

# Check if image exists locally
docker images | grep devops-static-site
```

**Problem:** ImagePullBackOff error
```bash
# Ensure imagePullPolicy is set to Never in deployment.yaml
# Rebuild image with correct tag
docker build -t devops-static-site:v1 app/
```

### Jenkins Issues

**Problem:** Jenkins can't access Docker
```bash
# Verify Docker socket mount
docker exec jenkins docker ps

# Verify workspace mount
docker exec jenkins ls /workspace
```

**Problem:** kubectl not found
```bash
# Verify kubectl in Jenkins
docker exec jenkins kubectl version --client

## Submission Checklist
- [x] GitHub repository created
- [x] All files committed
- [x] Docker Compose working (2+ services)
- [x] Kubernetes deployment successful
- [x] Jenkins pipeline running
- [x] README.md complete

## License
Educational project for DevOps learning.


