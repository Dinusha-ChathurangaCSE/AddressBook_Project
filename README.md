# AddressBook Project

An end-to-end deployment project for a Vaadin-based address book application. This project demonstrates a complete DevOps pipeline including infrastructure provisioning, CI/CD, containerization, and Kubernetes deployment.

## Description

This is a simple address book application built using the Vaadin framework. It allows users to manage contacts with basic CRUD operations, including adding, editing, deleting, and filtering contacts. The application is packaged as a WAR file and deployed in a Tomcat container.

The project includes a full DevOps setup with:
- Infrastructure as Code using Terraform
- Continuous Integration and Deployment with Jenkins
- Code quality analysis with SonarQube
- Artifact management with Nexus
- Containerization with Docker
- Orchestration with Kubernetes
- Configuration management with Ansible

## Features

- Add, edit, delete, and view contacts
- Filter contacts by name
- Responsive UI using Vaadin components
- In-memory data storage (demo service)
- Containerized deployment
- Automated CI/CD pipeline

## Tech Stack

- **Frontend/Backend**: Vaadin 8.0.0.alpha2 (Java framework)
- **Build Tool**: Maven
- **Containerization**: Docker (Tomcat 9 with JRE 11)
- **Infrastructure**: Terraform (AWS EC2)
- **CI/CD**: Jenkins
- **Code Quality**: SonarQube
- **Artifact Repository**: Nexus
- **Deployment**: Ansible, Kubernetes
- **Cloud**: AWS (EC2, ECR)

## Prerequisites

- Java 11 or higher
- Maven 3.x
- Docker
- AWS CLI configured with appropriate permissions
- Terraform
- Ansible
- kubectl (for Kubernetes deployment)
- Access to AWS services (EC2, ECR)
- Jenkins server with required plugins (Maven, SonarQube, Nexus, Docker, Terraform)

## Local Setup

1. Clone the repository:
   ```bash
   git clone https://github.com/Dinusha-ChathurangaCSE/AddressBook_Project.git
   cd AddressBook_Project
   ```

2. Build the project:
   ```bash
   mvn clean package
   ```

3. Run locally (optional, for development):
   - Deploy the WAR file to a Tomcat server or use an IDE to run the Vaadin application.

4. Build Docker image:
   ```bash
   docker build -t addressbook:latest .
   ```

5. Run the container:
   ```bash
   docker run -p 8080:8080 addressbook:latest
   ```

   Access the application at `http://localhost:8080/addressbook-2.0/`

## Infrastructure Setup

The project uses Terraform to provision AWS EC2 instances for Nexus, SonarQube, and testing environments.

1. Navigate to the Terraform directory:
   ```bash
   cd ec2_instance
   ```

2. Initialize Terraform:
   ```bash
   terraform init
   ```

3. Plan the deployment:
   ```bash
   terraform plan
   ```

4. Apply the configuration:
   ```bash
   terraform apply
   ```

## CI/CD Pipeline

The Jenkins pipeline automates the following stages:
- Code checkout from GitHub
- SonarQube code analysis
- Maven build and packaging
- Artifact upload to Nexus
- Docker image build and push to AWS ECR
- Infrastructure provisioning with Terraform
- Application deployment with Ansible

### Jenkins Setup

1. Install Jenkins on an EC2 instance using the provided script:
   ```bash
   ./scripts/jenkins-install.sh
   ```

2. Configure Jenkins with required credentials and tools.

3. Create a new pipeline job using the `JenkinsFile/cicdpipeline` script.

## Deployment

### Ansible Deployment

The Ansible playbooks install necessary tools (Docker, Git, Maven) on target hosts.

Run the playbook:
```bash
ansible-playbook -i inventory playbook.yaml
```

### Kubernetes Deployment

The `Application.yaml` file defines a Kubernetes Deployment and Service for the application.

1. Apply the Kubernetes manifests:
   ```bash
   kubectl apply -f Application.yaml
   ```

2. The application will be exposed via a LoadBalancer service on port 80, forwarding to container port 8080.

## Project Structure

```
AddressBook_Project/
├── src/
│   └── main/java/com/vaadin/tutorial/addressbook/
│       ├── AddressbookUI.java          # Main UI class
│       ├── ContactForm.java            # Contact form component
│       └── backend/
│           ├── Contact.java            # Contact model
│           └── ContactService.java     # Service layer
├── ec2_instance/                       # Terraform module for EC2
├── JenkinsFile/                        # Jenkins pipeline scripts
├── scripts/                            # Installation scripts
├── Dockerfile                          # Docker configuration
├── Application.yaml                    # Kubernetes manifests
├── pom.xml                             # Maven configuration
├── main.tf                             # Terraform root config
├── variables.tf                        # Terraform variables
├── playbook.yaml                       # Ansible playbook
└── README.md                           # This file
```

## Contributing

1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request

## License

This project is for educational purposes. Please check individual component licenses.
