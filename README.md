
# Project2-Compose

This repository contains the Docker Compose setup for a Jenkins CI/CD pipeline with Docker-in-Docker (DinD) support. The pipeline automates building, testing, and securing Node.js applications using Snyk for vulnerability scans.

## Table of Contents
- [Overview](#overview)
- [Technologies Used](#technologies-used)
- [Setup Instructions](#setup-instructions)
- [Pipeline Stages](#pipeline-stages)
- [Logging and Monitoring](#logging-and-monitoring)
- [Running the Application](#running-the-application)
- [Security and Vulnerability Scan](#security-and-vulnerability-scan)
- [Contributing](#contributing)
- [License](#license)

## Overview
This project provides a Docker Compose setup for running Jenkins with Docker-in-Docker (DinD) to automate the Continuous Integration (CI) and Continuous Delivery (CD) pipeline for a Node.js application. The pipeline pulls the project from GitHub, installs dependencies, runs tests, performs security scans using Snyk, and archives logs.

## Technologies Used
- Docker
- Jenkins (with Docker support)
- Node.js
- Snyk for security scanning

## Setup Instructions

### Prerequisites
- Docker and Docker Compose installed on your machine.
- A GitHub account with access to your Node.js application repository.

### Clone the Repository
Clone this repository to your local machine:

```bash
git clone https://github.com/Dawoodfazal319/Project2-Compose.git
cd Project2-Compose
```

### Docker Compose Setup
The `docker-compose.yml` file defines two services:
- Jenkins: Jenkins instance with support for Docker.
- DinD (Docker-in-Docker): A containerized Docker engine to allow Jenkins to run Docker commands within the pipeline.

To start the services, run:

```bash
docker-compose up -d
```

This command will start both Jenkins and DinD. Jenkins will be accessible at `http://localhost:8080`.

### Initial Jenkins Setup
Once Jenkins is running, follow these steps:
1. Retrieve the Jenkins initial admin password using the following command:
   ```bash
   docker exec -it jenkins cat /var/jenkins_home/secrets/initialAdminPassword
   ```
2. Paste the password into the Jenkins setup wizard at `http://localhost:8080`.
3. Install recommended plugins and set up your admin user account.

## Pipeline Stages

### 1. Install Dependencies
The pipeline installs Node.js dependencies by running:
```bash
npm install
```

### 2. Start Application
The application is started using:
```bash
npm start
```

### 3. Run Tests
The pipeline runs any defined tests in the project:
```bash
npm test
```

### 4. Security Scan
Snyk is used to scan for vulnerabilities in the application’s dependencies. The `Snyk API token` is stored in Jenkins credentials, and the following commands are run:
```bash
npm install -g snyk
snyk auth $SNYK_TOKEN
snyk test
```

## Logging and Monitoring
Logs from each pipeline stage, including build, test, and security scans, are archived for monitoring and troubleshooting. You can access logs directly in Jenkins under the **Console Output** section for each build, or from the archived artifacts.

## Running the Application
After setting up the Jenkins pipeline, you can trigger builds from Jenkins. Each build will:
- Pull the latest code from GitHub.
- Install dependencies.
- Run tests.
- Perform a security scan using Snyk.
- Start the Node.js application.

## Security and Vulnerability Scan
The pipeline integrates Snyk for security scanning, automatically checking the project for known vulnerabilities in dependencies. You must configure Snyk in Jenkins using the `SNYK_TOKEN` stored as credentials.

To authenticate with Snyk, the following commands are executed:
```bash
snyk auth $SNYK_TOKEN
snyk test
```

## Contributing
Feel free to contribute to this repository by submitting a pull request. Please make sure your changes align with the overall project goal and adhere to best practices.

