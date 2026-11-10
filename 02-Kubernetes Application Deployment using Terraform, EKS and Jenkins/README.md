# Kubernetes Application Deployment Using Terraform, Amazon EKS and Jenkins

**AWS | Terraform | Jenkins | Kubernetes | Docker | Amazon EKS | Amazon EC2 | Amazon S3 | GitHub**

## 1. Project Overview

This project demonstrates the automated deployment of a containerized Nginx web application on Amazon Elastic Kubernetes Service (EKS), using Terraform for Infrastructure as Code (IaC) and Jenkins for CI/CD automation.

The project automates AWS infrastructure provisioning, Kubernetes cluster creation, and application deployment through a Jenkins pipeline.

Terraform is used to provision the AWS infrastructure, including the Jenkins EC2 instance, networking components, and Amazon EKS cluster.

Jenkins executes the deployment pipeline, which provisions the EKS cluster and deploys the Nginx application using Kubernetes manifests.

The application is exposed externally through a Kubernetes LoadBalancer service, allowing users to access the Nginx web server using its load balancer DNS endpoint.

### Project Objectives

- Automate AWS infrastructure provisioning using Terraform.
- Deploy and configure a Jenkins server on Amazon EC2.
- Create an Amazon EKS cluster using Terraform.
- Automate Kubernetes application deployment using Jenkins.
- Deploy an Nginx web server using Kubernetes manifests.
- Expose the application through a Kubernetes LoadBalancer service.
- Validate the application deployment through a web browser.

## 2. Problem Statement

Traditionally, creating cloud infrastructure and deploying applications involves several manual steps.

Administrators must manually provision EC2 instances, configure networking, create Kubernetes clusters, and deploy application workloads.

This approach introduces challenges such as:

- **Manual infrastructure provisioning:** Creating AWS resources manually requires significant effort.
- **Deployment complexity:** Kubernetes cluster creation and application deployment involve multiple configuration steps.
- **Configuration inconsistencies:** Manual configuration can result in differences between environments.
- **Limited automation:** Repeating deployment steps manually increases operational overhead.
- **Infrastructure management:** Tracking and maintaining cloud resources becomes difficult without Infrastructure as Code.

### Proposed Solution

This project implements an automated infrastructure provisioning and application deployment workflow.

Terraform manages AWS infrastructure, Jenkins automates the deployment process, and Amazon EKS provides Kubernetes container orchestration.

Using this approach, the EKS cluster and Nginx application can be deployed through an automated Jenkins pipeline.

## 3. Architecture Diagram

The following diagram illustrates the end-to-end workflow
of AWS infrastructure provisioning using Terraform and
automated Kubernetes application deployment using Jenkins.

![Terraform EKS Jenkins Architecture](screenshot/terraform-eks-jenkins-architecture.png)


### Architecture Workflow

1. The developer pushes the Jenkinsfile, Terraform configurations, and Kubernetes manifests to GitHub.
2. Jenkins retrieves the source code from the GitHub repository.
3. Jenkins executes the pipeline stages.
4. Terraform provisions the AWS infrastructure and Amazon EKS cluster.
5. Jenkins updates its Kubernetes configuration using AWS CLI.
6. Jenkins executes Kubernetes deployment commands.
7. Kubernetes creates an Nginx deployment.
8. A Kubernetes LoadBalancer service exposes the Nginx application.
9. AWS provisions the associated load balancer.
10. Users access the Nginx application through the load balancer DNS endpoint.

## 4. AWS Services Used

| AWS Service | Purpose |
|---|---|
| Amazon EC2 | Hosts the Jenkins automation server |
| Amazon EKS | Provides managed Kubernetes cluster infrastructure |
| Amazon VPC | Provides networking infrastructure |
| Amazon S3 | Stores Terraform remote state |
| AWS IAM | Manages access permissions for AWS resources |
| Elastic Load Balancing | Exposes the Nginx application externally |
| Amazon EBS | Provides storage for EC2 and Kubernetes workloads where configured |

## 5. DevOps Tools Used

| Tool | Purpose |
|---|---|
| Terraform | Infrastructure as Code and AWS resource provisioning |
| Jenkins | CI/CD pipeline automation |
| Git | Source code version control |
| GitHub | Stores project source code and Jenkins pipeline |
| Kubernetes | Container orchestration |
| Amazon EKS | Managed Kubernetes service |
| kubectl | Kubernetes cluster management |
| AWS CLI | AWS resource and EKS configuration |
| Linux | Jenkins server administration |
| Nginx | Web server application |

## 6. Implementation Steps

### Step 1: Create an AWS EC2 Key Pair

- Open the AWS Management Console.
- Navigate to EC2.
- Select Key Pairs.
- Create a new key pair.
- Download and securely store the `.pem` file.

The key pair is used to securely connect to the Jenkins EC2 instance.

### Step 2: Create an Amazon S3 Bucket

Create an S3 bucket to store Terraform state files.

Configure the Terraform S3 backend to use this bucket.

The remote state allows Terraform to maintain infrastructure state separately from the local project files.

### Step 3: Provision the Jenkins Server Using Terraform

Create the first Terraform directory:

`1-terraform-jenkins-server/`

Include the required Terraform configuration files:

- `provider.tf`
- `variables.tf`
- `terraform.tfvars`
- `backend.tf`
- `server.tf`
- `vpc.tf`
- `route.tf`
- `security.tf`
- `outputs.tf`
- `jenkins-script.sh`

These files define the AWS provider, variables, backend configuration, networking, security groups and Jenkins EC2 instance.

Initialize and provision the infrastructure:

```bash
terraform init
terraform plan
terraform apply
```

After successful provisioning, note the Jenkins EC2 instance public IP address.

### Step 4: Install and Configure Jenkins

Access the Jenkins server through a browser using its configured endpoint.

Connect to the EC2 instance through SSH:

```bash
ssh -i "jenkins-server.pem" ec2-user@<PUBLIC_IP>
```

Retrieve the initial Jenkins administrator password:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Complete the Jenkins initial setup:

- Enter the administrator password.
- Install suggested plugins.
- Create an administrator account.
- Configure the Jenkins URL.

### Step 5: Configure Jenkins Credentials

Configure the credentials needed for the pipeline to access GitHub and AWS.

The tutorial uses:

- GitHub credentials.
- AWS Access Key ID.
- AWS Secret Access Key.

Store credentials securely in Jenkins Credentials Manager.

**Security recommendation:** Prefer an EC2 instance IAM role with least-privilege permissions instead of long-lived AWS access keys. Never commit credentials to GitHub.

### Step 6: Create Amazon EKS Infrastructure Using Terraform

Create the second Terraform directory:

`2-terraform-eks-deployment/`

Include:

- `backend.tf`
- `eks.tf`
- `vpc.tf`
- `variables.tf`
- `terraform.tfvars`

Use the Terraform AWS VPC and EKS modules to provision:

- VPC networking.
- Public and private subnets.
- NAT Gateway, where configured.
- Amazon EKS cluster.
- Managed node groups.

Configure the AWS region and cluster variables according to your environment.

### Step 7: Create Kubernetes Deployment Files

Create a directory:

`kubernetes/`

Add the following files:

**nginx-deployment.yaml**

This file defines the Kubernetes Deployment for the Nginx web server.

It specifies the Nginx container image, container port and replica configuration.

**nginx-service.yaml**

This file defines the Kubernetes Service.

Configure the service with:

- Service type: LoadBalancer
- Protocol: TCP
- Service port: 80
- Target port: 80

This exposes the Nginx application outside the Kubernetes cluster through AWS load balancing.

### Step 8: Create the Jenkinsfile

Create a `Jenkinsfile` in the root project directory.

The Jenkins pipeline contains two main stages:

**Stage 1: Create an EKS Cluster**

- Navigate to the Terraform EKS directory.
- Initialize Terraform.
- Apply the Terraform configuration.
- Provision the Amazon EKS cluster and required infrastructure.

**Stage 2: Deploy to EKS**

- Update Kubernetes configuration using AWS CLI.
- Connect Jenkins to the EKS cluster.
- Apply the Nginx deployment manifest.
- Apply the Nginx service manifest.

### Step 9: Push Project Files to GitHub

Create a GitHub repository for the project.

Upload the following files and directories:

```text
jenkins-pipeline/
│
├── Jenkinsfile
│
├── 2-terraform-eks-deployment/
│   ├── backend.tf
│   ├── eks.tf
│   ├── vpc.tf
│   ├── variables.tf
│   └── terraform.tfvars
│
└── kubernetes/
    ├── nginx-deployment.yaml
    └── nginx-service.yaml
```

Commit and push the files to the main branch.

### Step 10: Create a Jenkins Pipeline Job

- Open Jenkins.
- Select New Item.
- Enter a pipeline job name.
- Select Pipeline.
- Choose Pipeline Script from SCM.
- Select Git as the source control system.
- Enter the GitHub repository URL.
- Configure the required repository credentials.
- Specify the main branch.
- Save the pipeline configuration.

### Step 11: Execute the Jenkins Pipeline

Open the Jenkins pipeline job and select Build Now.

Jenkins automatically:

1. Retrieves the project from GitHub.
2. Executes Terraform initialization.
3. Provisions the EKS cluster.
4. Updates Kubernetes configuration.
5. Deploys the Nginx application.
6. Creates the LoadBalancer service.

Monitor the pipeline through Jenkins Console Output.

### Step 12: Verify the Application Deployment

Open the AWS Management Console.

Navigate to Amazon EKS and verify that the cluster is active.

Check Kubernetes resources:

```bash
kubectl get nodes
kubectl get pods
kubectl get deployments
kubectl get services
```

Locate the external load balancer endpoint:

```bash
kubectl get svc
```

Copy the external DNS address of the Nginx service and open it in a browser.

The Nginx welcome page should appear if the deployment and network configuration are successful.

## Results and Key Learnings

### Results

The intended outcomes of this project are:

- Provisioned AWS infrastructure using Terraform.
- Configured a Jenkins server on Amazon EC2.
- Automated EKS cluster provisioning through a Jenkins pipeline.
- Deployed an Nginx application using Kubernetes manifests.
- Exposed the application using a Kubernetes LoadBalancer service.
- Validated application accessibility using the load balancer DNS endpoint.

Only list the outcomes that you have successfully verified.

### Key Learnings

**Infrastructure as Code**

Learned how Terraform manages AWS infrastructure and provisions resources in a repeatable manner.

**AWS Cloud Infrastructure**

Gained practical understanding of EC2, VPC, IAM, S3 and Amazon EKS.

**Jenkins CI/CD Automation**

Learned how Jenkins pipelines automate infrastructure creation and Kubernetes application deployment.

**Kubernetes Orchestration**

Developed an understanding of Kubernetes Deployments, Pods, Services and application exposure.

**Terraform Remote State**

Learned how Terraform uses an S3 backend to store infrastructure state.

**AWS and Kubernetes Integration**

Understood how AWS CLI and kubectl connect Jenkins with Amazon EKS.

**Application Deployment and Validation**

Learned how to expose an application through a LoadBalancer service and validate it through a browser.

**Troubleshooting**

Gained experience investigating Terraform provisioning errors, Jenkins pipeline failures, AWS permission issues and Kubernetes deployment problems.

## Cleanup and Resource Management

AWS resources may incur charges while running.

After completing the project, remove the resources that are no longer needed.

Destroy the EKS infrastructure first, from the appropriate Terraform directory:

```bash
terraform init
terraform destroy
```

Then destroy the Jenkins server infrastructure from its Terraform directory:

```bash
terraform init
terraform destroy
```

Verify that dependent resources such as load balancers, NAT Gateways, EKS node groups and EC2 instances have been removed.

Retain or remove the S3 Terraform state bucket according to your backup and project requirements.