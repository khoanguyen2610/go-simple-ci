# Go Simple CI

A simple HTTP service written in Go with a health check endpoint, Docker support, and GitHub Actions CI/CD pipeline for AWS EC2 deployment.

## Features

- HTTP service running on port 8888
- GET `/health` endpoint returning JSON health status
- Docker containerization
- Docker Compose support
- GitHub Actions CI/CD pipeline
- Automated deployment to AWS EC2

## Local Development

### Prerequisites

- Go 1.24.1 or later
- Docker and Docker Compose

### Running Locally

1. **Run with Go:**
   ```bash
   go run main.go
   ```

2. **Run with Docker:**
   ```bash
   docker build -t go-simple-ci .
   docker run -p 8888:8888 go-simple-ci
   ```

3. **Run with Docker Compose:**
   ```bash
   docker-compose up --build
   ```

### Testing

Run the tests:
```bash
go test -v ./...
```

### Health Check

Once the service is running, you can check the health endpoint:
```bash
curl http://localhost:8888/health
```

Expected response:
```json
{
  "status": "healthy",
  "timestamp": "2025-09-17T10:30:45Z",
  "message": "Service is running successfully"
}
```

## Deployment

### GitHub Actions Setup

The project includes a GitHub Actions workflow that:
1. Runs tests and code quality checks
2. Builds and pushes Docker image to Docker Hub
3. Deploys to AWS EC2

### Required GitHub Secrets

Set up the following secrets in your GitHub repository:

| Secret Name | Description |
|-------------|-------------|
| `DOCKER_USERNAME` | Your Docker Hub username |
| `DOCKER_PASSWORD` | Your Docker Hub password or access token |
| `EC2_HOST` | Your EC2 instance public IP or hostname |
| `EC2_USER` | SSH username (usually `ec2-user` or `ubuntu`) |
| `EC2_SSH_KEY` | Private SSH key for EC2 access |
| `EC2_PORT` | SSH port (optional, defaults to 22) |

### EC2 Prerequisites

Your EC2 instance should have:
1. Docker installed
2. SSH access configured
3. Security group allowing inbound traffic on port 8888

### Manual Deployment

If you want to deploy manually to your EC2 instance:

1. **Build and push image:**
   ```bash
   docker build -t your-username/go-simple-ci .
   docker push your-username/go-simple-ci
   ```

2. **Deploy to EC2:**
   ```bash
   ssh your-ec2-user@your-ec2-host
   docker pull your-username/go-simple-ci
   docker stop go-simple-ci || true
   docker rm go-simple-ci || true
   docker run -d --name go-simple-ci --restart unless-stopped -p 8888:8888 your-username/go-simple-ci
   ```

## Project Structure

```
.
├── main.go              # Main HTTP service
├── main_test.go         # Unit tests
├── go.mod              # Go module file
├── go.sum              # Go dependencies
├── Dockerfile          # Docker build instructions
├── docker-compose.yml  # Docker Compose configuration
├── .dockerignore       # Docker ignore file
├── .github/
│   └── workflows/
│       └── ci-cd.yml   # GitHub Actions workflow
└── README.md           # This file
```

## API Documentation

### GET /health

Returns the health status of the service.

**Response:**
- Status Code: 200 OK
- Content-Type: application/json

**Response Body:**
```json
{
  "status": "healthy",
  "timestamp": "2025-09-17T10:30:45Z",
  "message": "Service is running successfully"
}
```

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Run tests: `go test -v ./...`
5. Submit a pull request

## License

This project is open source and available under the MIT License.
