# DevSecOps Three-Tier Application Deployment on AWS EKS

**End-to-End DevSecOps | AWS | Kubernetes | Jenkins | Terraform | Argo CD | SonarQube | Trivy | Prometheus | Grafana**

## 1. Project Overview

This project demonstrates the deployment of a containerized three-tier web application on Amazon Elastic Kubernetes Service (EKS), using DevSecOps practices to integrate continuous integration, security scanning, GitOps-based continuous delivery, and infrastructure monitoring.

The application architecture consists of three layers:

- **Frontend:** React.js application providing the user interface.
- **Backend:** Node.js application handling business logic and API requests.
- **Database:** MongoDB for application data storage.

Jenkins automates application builds, code analysis, vulnerability scanning, and Docker image publishing. Amazon ECR stores container images, while Argo CD manages Kubernetes deployments through GitOps.

Prometheus and Grafana provide monitoring and visualization of infrastructure and Kubernetes metrics.

### Project Objectives

- Automate application build and deployment workflows.
- Deploy containerized applications on Amazon EKS.
- Implement security scanning within the CI pipeline.
- Manage cloud infrastructure using Infrastructure as Code.
- Implement GitOps-based continuous delivery using Argo CD.
- Monitor Kubernetes workloads using Prometheus and Grafana.

## 2. Problem Statement

Traditional application deployment processes often involve manual infrastructure configuration, application builds, security testing, and deployment activities.

These manual processes can introduce several challenges:

- **Deployment inconsistencies:** Different environments may have different configurations.
- **Security risks:** Vulnerabilities may remain undetected without automated security scanning.
- **Time-consuming deployments:** Manual deployment steps increase operational effort.
- **Infrastructure management complexity:** Creating and maintaining cloud resources manually can lead to configuration drift.
- **Limited monitoring:** Without centralized metrics, identifying infrastructure and application issues becomes difficult.

### Proposed Solution

This project uses an automated DevSecOps workflow to address these challenges.

Terraform is used for infrastructure provisioning where configured, Jenkins handles CI automation, SonarQube and Trivy provide security and quality checks, and Docker packages application components into container images.

Amazon EKS orchestrates the application containers, Argo CD synchronizes Kubernetes manifests from Git, and Prometheus and Grafana provide infrastructure monitoring.

## 3. Architecture Diagram

The following diagram illustrates the end-to-end DevSecOps
workflow, including CI/CD automation, security scanning,
GitOps deployment, AWS EKS, and application monitoring.

![AWS DevSecOps Architecture](screenshot/devsecops-architecture.png)

### Architecture Workflow

1. Developers push application code to GitHub.
2. Jenkins executes the configured CI pipeline.
3. SonarQube performs code quality analysis.
4. Dependency and container security scans identify potential vulnerabilities.
5. Docker builds frontend and backend container images.
6. Container images are pushed to Amazon ECR.
7. Kubernetes manifest image references are updated in Git.
8. Argo CD detects the desired configuration and synchronizes it with Amazon EKS.
9. Kubernetes runs the frontend, backend, and database components.
10. Prometheus collects metrics and Grafana displays monitoring dashboards.

## 4. AWS Services Used

| AWS Service            | Purpose                                                                     |
| ---------------------- | --------------------------------------------------------------------------- |
| Amazon EC2             | Hosts Jenkins and supporting DevOps services                                |
| Amazon EKS             | Runs and manages Kubernetes workloads                                       |
| Amazon ECR             | Stores frontend and backend Docker images                                   |
| Amazon VPC             | Provides networking for AWS infrastructure                                  |
| AWS IAM                | Manages permissions for infrastructure and deployment operations            |
| Elastic Load Balancing | Exposes application services and distributes incoming traffic               |
| Amazon S3              | Stores Terraform state if remote state is configured                        |
| Amazon DynamoDB        | Used for Terraform state locking if configured with the applicable backend  |


## 5. DevOps Tools Used

| Tool                   | Purpose                                         |
| ---------------------- | ----------------------------------------------- |
| Git and GitHub         | Source code and configuration version control   |
| Terraform              | Infrastructure provisioning                     |
| Jenkins                | Continuous integration automation               |
| Docker                 | Application containerization                    |
| Kubernetes             | Container orchestration                         |
| kubectl                | Kubernetes cluster management                   |
| eksctl                 | EKS cluster creation and management, if used    |
| SonarQube              | Static code quality and security analysis       |
| Trivy                  | Container and filesystem vulnerability scanning |
| OWASP Dependency-Check | Third-party dependency vulnerability scanning   |
| Argo CD                | GitOps-based continuous delivery                |
| Helm                   | Kubernetes package management                   |
| Prometheus             | Metrics collection                              |
| Grafana                | Monitoring dashboards                           |
| AWS CLI                | AWS resource configuration and management       |

## 6. Implementation Steps

### Step 1: Configure AWS Access

- Set up AWS IAM permissions required for the project.
- Configure AWS CLI authentication.
- Verify access to the intended AWS account and region.
- Follow least-privilege practices and avoid committing AWS credentials.

### Step 2: Provision the Jenkins Server

- Create an EC2 instance for Jenkins.
- Use Terraform for EC2 provisioning if configured in the project.
- Install Java and Jenkins.
- Configure the required security groups.
- Access the Jenkins web interface and complete initial setup.

### Step 3: Install DevOps Tools

Install and configure the tools required by the CI/CD workflow:

- Docker
- Terraform
- AWS CLI
- kubectl
- eksctl
- Trivy
- SonarQube Scanner
- OWASP Dependency-Check

Configure the necessary Jenkins plugins and credentials.

### Step 4: Create the Amazon EKS Cluster

- Create the EKS cluster using the infrastructure method implemented in the project.
- Configure managed node groups and networking.
- Update the local Kubernetes configuration.
- Verify that worker nodes are registered and ready.

Example verification:

```bash
kubectl get nodes
```

### Step 5: Configure Amazon ECR

- Create private ECR repositories for frontend and backend images.
- Configure Jenkins authentication to ECR.
- Build Docker images for the application components.
- Push versioned images to the appropriate repositories.

### Step 6: Configure Security Scanning

Integrate security and quality checks into the Jenkins pipeline:

- SonarQube for source code analysis.
- Trivy for vulnerability scanning.
- OWASP Dependency-Check for dependency analysis, if implemented.

### Step 7: Configure Jenkins Pipelines

Create or configure Jenkins pipelines for frontend and backend applications.

The pipeline workflow includes:

1. Checkout source code.
2. Install dependencies and run configured tests.
3. Perform code quality analysis.
4. Run security scanning.
5. Build Docker images.
6. Authenticate to Amazon ECR.
7. Push images to ECR.
8. Update Kubernetes image references in Git if the pipeline implements this step.

### Step 8: Install and Configure Argo CD

- Install Argo CD in the Kubernetes cluster.
- Configure repository access.
- Create Argo CD applications for the relevant Kubernetes manifests.
- Configure synchronization settings.
- Verify application synchronization and health.

### Step 9: Deploy the Three-Tier Application

Deploy the application components using Kubernetes manifests managed through Argo CD.

Application components include:

- MongoDB database
- Node.js backend
- React.js frontend
- Services and ingress resources, where configured

Verify application pod status:

```bash
kubectl get pods -A
kubectl get services -A
kubectl get ingress -A
```

### Step 10: Configure Persistent Storage

- Configure PersistentVolumeClaims for database storage where required.
- Confirm that the storage class supports the requested volume.
- Verify that database storage is bound and mounted correctly.

### Step 11: Configure Monitoring

- Install Prometheus and Grafana using Helm.
- Configure metrics collection for Kubernetes components.
- Access the Grafana dashboard.

### Step 12: Validate the Deployment

- Verify successful Jenkins pipeline execution.
- Confirm frontend and backend images are available in ECR.
- Check that Kubernetes workloads are healthy.
- Confirm Argo CD reports the intended synchronization status.
- Access the deployed application through its configured endpoint.
- Verify monitoring dashboards display metrics.

## Results and Key Learnings

### Results

The intended project outcomes are:

- Automated application build and container image publishing using Jenkins.
- Integration of source code analysis and vulnerability scanning into the CI pipeline.
- Deployment of a containerized three-tier application on Amazon EKS.
- Container image storage and version management using Amazon ECR.
- GitOps-based application delivery through Argo CD.
- Kubernetes infrastructure monitoring using Prometheus and Grafana.
- Repeatable infrastructure configuration using Terraform where implemented.

### Key Learnings

**AWS Infrastructure Management**

Gained practical understanding of configuring AWS resources, IAM permissions, networking and Kubernetes infrastructure.

**CI/CD Automation**

Learned how Jenkins pipelines coordinate code checkout, security checks, Docker image builds and image publishing.

**Containerization and Kubernetes**

Developed experience with Docker images, Kubernetes deployments, services, namespaces, ingress and persistent storage.

**DevSecOps Practices**

Explored the integration of SonarQube, Trivy and dependency scanning into automated build workflows.

**GitOps Deployment**

Learned how Argo CD manages Kubernetes application configuration using Git as the desired-state source.

**Monitoring and Observability**

Explored Prometheus metrics collection and Grafana dashboards for Kubernetes resource monitoring.

**Troubleshooting**

Practiced investigating pipeline failures, image-pull errors, pod readiness issues, deployment synchronization problems and cloud permission errors.