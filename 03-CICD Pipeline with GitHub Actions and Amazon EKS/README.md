# GitHub Actions CI/CD Pipeline with Amazon EKS

**AWS | GitHub Actions | Terraform | Docker | Kubernetes | Amazon EKS | SonarQube | Trivy | Docker Hub **

## 1. Project Overview

This project demonstrates the implementation of an automated CI/CD pipeline using GitHub Actions to build, test, scan, containerize, and deploy a Tic-Tac-Toe web application on Amazon Elastic Kubernetes Service (EKS).

The project integrates GitHub Actions with a self-hosted runner hosted on an Amazon EC2 instance to automate application deployment.

Terraform is used to provision the AWS infrastructure and Amazon EKS cluster. Docker is used to containerize the application, while Docker Hub acts as the container image registry.

SonarQube performs code quality analysis, and Trivy scans Docker images for security vulnerabilities.

After successful pipeline execution, the application is deployed to Amazon EKS using Kubernetes configurations.


### Project Objectives

- Implement automated CI/CD using GitHub Actions.
- Configure a self-hosted GitHub Actions runner on Amazon EC2.
- Provision Amazon EKS infrastructure using Terraform.
- Integrate SonarQube for code quality analysis.
- Integrate Trivy for container vulnerability scanning.
- Containerize the application using Docker.
- Push Docker images to Docker Hub.
- Deploy the application on Amazon EKS.

## 2. Problem Statement

Traditional application deployment processes involve several manual activities, including source code validation, application building, security testing, container image creation, and Kubernetes deployment.

These manual activities can create challenges such as:

- **Manual deployment:** Repeated manual deployment steps increase operational effort.
- **Slow release cycles:** Manual testing and deployment delay application delivery.
- **Security vulnerabilities:** Without automated security scanning, vulnerable dependencies and container images may go unnoticed.
- **Infrastructure management:** Manually creating and maintaining Kubernetes infrastructure is complex.
- **Deployment inconsistencies:** Different deployment configurations can cause unexpected application behavior.
- **Limited pipeline visibility:** Without notifications, developers may not immediately know whether a deployment succeeded or failed.

### Proposed Solution

This project implements an automated CI/CD workflow using GitHub Actions and Amazon EKS.

Whenever application code is updated in GitHub, the configured workflow performs the required build, testing, code quality analysis, and security scanning stages.

Docker packages the application into a container image and pushes it to Docker Hub.

Terraform manages the AWS infrastructure, while Kubernetes and Amazon EKS provide the application deployment environment.

## 3. Architecture Diagram

The following diagram illustrates the end-to-end GitHub Actions CI/CD workflow.

![GitHub Actions EKS Architecture]  (screenshot/github-actions-eks-architecture.png)

### Architecture Workflow

1. Developers push application code to GitHub.
2. GitHub Actions detects the configured repository event.
3. The workflow executes on the self-hosted EC2 runner.
4. The application is built and tested.
5. SonarQube performs code quality analysis.
6. Trivy scans the application or Docker image for vulnerabilities, according to the configured workflow.
7. Docker builds the application container image.
8. The image is pushed to Docker Hub.
9. Terraform provisions the required AWS infrastructure and Amazon EKS cluster.
10. Kubernetes deployment configurations are applied to Amazon EKS.
11. The application becomes accessible through its configured Kubernetes service or ingress endpoint.


## 4. AWS Services Used

| AWS Service            | Purpose                                                  |
| ---------------------- | -------------------------------------------------------- |
| Amazon EC2             | Hosts the self-hosted GitHub Actions runner              |
| Amazon EKS             | Provides managed Kubernetes infrastructure               |
| Amazon VPC             | Provides networking for AWS infrastructure               |
| AWS IAM                | Manages permissions for GitHub Actions and AWS resources |
| Elastic Load Balancing | Exposes the application externally, if configured        |
| Amazon EBS             | Provides persistent storage where configured             |

## 5. DevOps Tools Used

| Tool               | Purpose                                         |
| ------------------ | ----------------------------------------------- |
| Git                | Source code version control                     |
| GitHub             | Application source code and workflow management |
| GitHub Actions     | CI/CD workflow automation                       |
| Self-Hosted Runner | Executes GitHub Actions jobs on EC2             |
| Terraform          | Infrastructure as Code                          |
| Docker             | Application containerization                    |
| Docker Hub         | Container image registry                        |
| Kubernetes         | Container orchestration                         |
| Amazon EKS         | Managed Kubernetes service                      |
| kubectl            | Kubernetes cluster management                   |
| SonarQube          | Code quality and static analysis                |
| Trivy              | Container vulnerability scanning                |
| AWS CLI            | AWS resource and EKS management                 |

## 6. Implementation Steps

### Step 1: Create a GitHub Repository

- Create a GitHub repository for the Tic-Tac-Toe application.
- Upload the application source code.
- Organize the project files.
- Create the required GitHub Actions workflow directory.

Recommended workflow directory:

```text
.github/
└── workflows/
    └── ci-cd.yml
```

### Step 2: Create an Amazon EC2 Instance

- Open the AWS Management Console.
- Navigate to Amazon EC2.
- Launch an EC2 instance for the self-hosted GitHub Actions runner.
- Configure the required security groups.
- Connect to the EC2 instance using SSH.
- Install the required dependencies.

### Step 3: Configure the GitHub Actions Self-Hosted Runner

- Open the GitHub repository.
- Navigate to Settings.
- Select Actions.
- Open Runners.
- Select New self-hosted runner.
- Choose the appropriate operating system.
- Follow the GitHub-provided installation and configuration commands on EC2.
- Start the runner and verify that it appears as Online in GitHub.

### Step 4: Install Required DevOps Tools

Install and configure the required tools on the runner or their respective servers:

- Docker
- Terraform
- AWS CLI
- kubectl
- SonarQube
- Trivy

Configure the required permissions and environment variables.

### Step 5: Configure AWS Authentication

Configure GitHub Actions to authenticate with AWS.

The project may use AWS credentials stored as GitHub Actions secrets.

Recommended secret configuration:

- `AWS_ACCESS_KEY_ID`
- `AWS_SECRET_ACCESS_KEY`
- `AWS_REGION`

Use least-privilege IAM permissions.

**Security recommendation:** For a new implementation, prefer GitHub Actions OIDC with an AWS IAM role instead of long-lived access keys.

Never commit credentials or secret values to the repository.

### Step 6: Provision Amazon EKS Using Terraform

Create the Terraform configuration files required for AWS infrastructure provisioning.

The configuration should define the required resources:

- AWS VPC and subnets.
- IAM roles and policies.
- Amazon EKS cluster.
- EKS worker nodes or managed node groups.

Initialize Terraform:

```bash
terraform init
```

Review the infrastructure plan:

```bash
terraform plan
```

Provision the infrastructure:

```bash
terraform apply
```

Verify that the EKS cluster has been created successfully.

### Step 7: Configure SonarQube

- Install or configure SonarQube.
- Create a project for the application.
- Generate the required authentication token.
- Configure the SonarQube URL and token in GitHub Actions secrets.
- Integrate SonarQube analysis into the CI workflow.
- Review the code quality analysis results.

### Step 8: Configure Trivy Security Scanning

Install and configure Trivy.

Integrate Trivy into the GitHub Actions workflow to scan the application or Docker image.

Example command for scanning a Docker image:

```bash
trivy image <IMAGE_NAME>:<TAG>
```

Review the scan results and identify vulnerabilities classified as:

- CRITICAL
- HIGH
- MEDIUM
- LOW

Configure security gates according to the project's requirements.

### Step 9: Build the Docker Image

Create or verify the Dockerfile for the Tic-Tac-Toe application.

Build the container image:

```bash
docker build -t <IMAGE_NAME>:latest .
```

Verify the image:

```bash
docker images
```

### Step 10: Push the Image to Docker Hub

- Create a Docker Hub account.
- Create a repository for the application image.
- Configure Docker Hub username and access token in GitHub Actions secrets.
- Authenticate with Docker Hub through the workflow.
- Tag the image.
- Push the image to Docker Hub.

Example:

```bash
docker tag <IMAGE_NAME>:latest <DOCKERHUB_USERNAME>/<REPOSITORY>:latest

docker push <DOCKERHUB_USERNAME>/<REPOSITORY>:latest
```

Use versioned image tags for repeatable deployments rather than relying only on `latest`.

### Step 11: Configure Kubernetes Deployment

Create the Kubernetes manifest files for the application.

Typical files include:

- `deployment.yaml`
- `service.yaml`

The Deployment defines:

- Application container image.
- Number of replicas.
- Container ports.
- Resource configurations.
- Health checks, where configured.

The Service exposes the application inside or outside the Kubernetes cluster, depending on its service type.

### Step 12: Deploy the Application to Amazon EKS

Configure Kubernetes access:

```bash
aws eks update-kubeconfig \
  --region <AWS_REGION> \
  --name <EKS_CLUSTER_NAME>
```

Verify cluster connectivity:

```bash
kubectl get nodes
```

Deploy the application:

```bash
kubectl apply -f kubernetes/deployment.yaml

kubectl apply -f kubernetes/service.yaml
```

Verify the deployment:

```bash
kubectl get pods
kubectl get deployments
kubectl get services
```
### Step 13: Execute and Validate the GitHub Actions Workflow

- Push a code change to the configured GitHub branch.
- Open the repository's Actions tab.
- Select the CI/CD workflow.
- Monitor the execution of each stage.
- Review build, testing and security scanning results.
- Verify Docker image publishing.
- Confirm successful Kubernetes deployment.

## Results and Key Learnings

### Results

The intended outcomes of this project include:

- Automated CI/CD workflow using GitHub Actions.
- Configured a self-hosted runner on Amazon EC2.
- Integrated SonarQube for code quality analysis.
- Integrated Trivy for container vulnerability scanning.
- Containerized the application using Docker.
- Published application images to Docker Hub.
- Provisioned Amazon EKS infrastructure using Terraform.
- Deployed the application using Kubernetes manifests.

### Key Learnings

**GitHub Actions Automation**

Learned how to configure automated workflows using GitHub Actions and manage CI/CD execution.

**Self-Hosted Runner Configuration**

Gained practical understanding of configuring and managing a self-hosted GitHub Actions runner on Amazon EC2.

**Infrastructure as Code**

Learned how Terraform provisions and manages AWS infrastructure.

**AWS and Kubernetes Integration**

Developed an understanding of Amazon EKS, IAM permissions, AWS CLI and Kubernetes cluster management.

**Containerization**

Learned how to package applications using Docker and publish images to Docker Hub.

**DevSecOps Practices**

Understood how SonarQube and Trivy integrate into CI/CD workflows to improve code quality and container security.

**Kubernetes Deployment**

Gained practical knowledge of Kubernetes Deployments, Pods, Services and application exposure.

**Troubleshooting**

Developed experience troubleshooting GitHub Actions workflow failures, runner connectivity, AWS permissions, Docker image publishing and Kubernetes deployment issues.

## Cleanup and Resource Management

AWS infrastructure may incur charges while resources are running.

After completing the project:

- Remove unused Kubernetes workloads.
- Delete unused load balancers.
- Stop or terminate the EC2 self-hosted runner when it is no longer needed.
- Destroy Terraform-managed infrastructure after checking dependencies.
- Remove unused Docker images where appropriate.
- Review GitHub Actions secrets and permissions.

If Terraform manages the EKS infrastructure, execute cleanup from the appropriate Terraform directory:

```bash
terraform init
terraform plan -destroy
terraform destroy
```

Verify that all chargeable AWS resources have been removed.