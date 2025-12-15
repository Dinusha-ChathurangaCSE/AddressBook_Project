# AddressBook Application - CI/CD Deployment to Kubernetes

This project implements a complete CI/CD pipeline for deploying an AddressBook application to Kubernetes using Jenkins, Terraform, Ansible, Docker, and AWS services.

## Project Overview

The deployment process is divided into three Jenkins pipelines that automate infrastructure provisioning, application building, and deployment to a Kubernetes cluster.

## Architecture

The project uses the following technologies:
- **Jenkins**: CI/CD orchestration
- **Terraform**: Infrastructure provisioning on AWS
- **Ansible**: Configuration management
- **Maven**: Build tool
- **SonarQube**: Code quality analysis
- **Nexus**: Artifact repository
- **Docker**: Containerization
- **AWS ECR**: Container registry
- **Amazon EKS**: Kubernetes cluster management

## Pipeline 1: Ansible Configuration

This pipeline configures test nodes using Ansible for automation.

### Stages:
1. **Checkout Repository**: Clones the project from GitHub
2. **Ansible Test**: 
   - Pings all hosts to verify connectivity
   - Executes the Ansible playbook for configuration

### Prerequisites:
- Ansible credentials stored in Jenkins as `ansiblekey`
- Inventory file with target hosts
- Ansible playbook (`playbook.yaml`)

## Pipeline 2: Infrastructure Provisioning

This pipeline provisions three AWS EC2 instances using Terraform: SonarQube server, Nexus repository, and a test server.

### Stages:
1. **Checkout Stage**: Clones the repository
2. **Terraform Init**: Initializes Terraform working directory
3. **Terraform Plan**: Creates execution plan
4. **Terraform Apply**: Provisions infrastructure automatically

### Prerequisites:
- AWS credentials stored in Jenkins as `aws-key`
- Terraform configuration files in the repository
- Proper AWS IAM permissions

## Pipeline 3: Complete CI/CD Pipeline

This is the main pipeline that builds, tests, and deploys the application to Kubernetes.

### Stages:

1. **Checkout Stage**: Retrieves source code from GitHub

2. **SonarQube Analysis**: Performs code quality and security scanning

3. **Maven Build**: Compiles and packages the application as a WAR file

4. **Nexus Upload**: Uploads the build artifact to Nexus repository
   - Artifact: `addressbook-2.0.war`
   - Repository: `maven-snapshots`

5. **Docker Build**: Creates a Docker image for the application

6. **ECR Push**: Pushes the Docker image to AWS ECR
   - Tags with both `latest` and build number
   - Region: `us-east-1`

7. **Kubernetes Deployment**: Deploys the application to EKS cluster
   - Updates kubeconfig for the EKS cluster
   - Applies Kubernetes manifests

### Pipeline Configuration:
- **Build Retention**: Keeps builds for 30 days, maximum 2 builds
- **Timeout**: 30 minutes
- **Maven Tool**: Configured in Jenkins

## Prerequisites

### Jenkins Configuration:
- **Credentials**:
  - `ansiblekey`: SSH private key for Ansible
  - `aws-key`: AWS access credentials
  - `nexus`: Nexus repository credentials

- **Tools**:
  - Maven installation named 'Maven'
  - SonarQube server named 'sonarqube-server'

- **Plugins**:
  - Pipeline
  - Git
  - SonarQube Scanner
  - Nexus Artifact Uploader
  - AWS CLI
  - Kubernetes CLI

### AWS Resources:
- EKS cluster: `super-project-jenkins`
- ECR repository: `super-project`
- IAM role with appropriate permissions

### Repository Structure:
```
.
├── Dockerfile
├── Application.yaml (Kubernetes manifest)
├── playbook.yaml (Ansible playbook)
├── inventory (Ansible inventory)
├── terraform configuration files
└── pom.xml (Maven configuration)
```

## Deployment Instructions

1. **Set up Infrastructure**:
   ```
   Run Pipeline 2 to provision AWS infrastructure
   ```

2. **Configure Servers**:
   ```
   Run Pipeline 1 to configure servers with Ansible
   ```

3. **Deploy Application**:
   ```
   Run Pipeline 3 to build and deploy the application
   ```

## Monitoring and Verification

After successful deployment:
- Check SonarQube dashboard for code quality metrics
- Verify artifacts in Nexus repository
- Check Docker images in AWS ECR
- Verify application pods in Kubernetes:
  ```bash
  kubectl get pods
  kubectl get services
  ```

## Post-Build Actions

The main pipeline includes post-build notifications:
- **Always**: Logs job completion
- **Success**: Logs successful deployment
- **Failure**: Logs failure for troubleshooting

## Troubleshooting

Common issues and solutions:
- **Ansible connection failures**: Verify SSH key and inventory configuration
- **Terraform errors**: Check AWS credentials and IAM permissions
- **Docker push failures**: Ensure ECR authentication is valid
- **Kubernetes deployment issues**: Verify kubeconfig and cluster access

## Repository

Source code: [https://github.com/Dinusha-ChathurangaCSE/AddressBook_Project.git](https://github.com/Dinusha-ChathurangaCSE/AddressBook_Project.git)

## Notes

- The application is built as a WAR file (`addressbook-2.0.war`)
- Docker images are tagged with both `latest` and the Jenkins build number for versioning
- The pipeline automatically handles authentication with AWS services
- All stages are executed sequentially with proper error handling