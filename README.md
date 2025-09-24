# Altschool-3rdsemester-assesment2

### URL LINK - http://a11afb3c98b144246b1e23591f81bbe4-38390223.eu-west-2.elb.amazonaws.com/
## Introduction

This Assesment details the deployment of a containerized retail application on ***Amazon Elastic Kubernetes Service (EKS)***, leveraging ***Terraform for Infrastructure as Code (IaC)***.

Key components include:

- Infrastructure provisioning and automation with Terraform
- EKS cluster creation and containerized application deployment
- CI/CD pipeline implementation for Terraform using GitHub Actions

## 1. Infrastructure Provisioning with Terraform

The infrastructure was provisioned using **Terraform**, organized with a clear, resource-based file structure for better readability and maintenance. Each key AWS resource is defined in its own file:

`vpc.tf` – VPC configuration and CIDR block

`subnets.tf` – Public and private subnets across availability zones

`igw.tf` – Internet Gateway setup for public access

`routes.tf` – Route tables and their associations

`eks.tf` – EKS cluster and node group provisioning

`providers.tf` – AWS provider setup

`locals.tf` – Reusable values and naming conventions

***A remote backend was also configured to manage the Terraform state file securely, enabling collaboration, ensuring consistency, and supporting disaster recovery.***


## 2. EKS Cluster Setup
The **Amazon EKS cluster** was created using the `eks.tf` definition. Key configurations included:  
- Worker node groups with auto-scaling  
- IAM roles and policies for EKS and worker nodes  
- Security groups for Kubernetes communication  

Once provisioned, the cluster configuration was updated locally with:  
```bash
aws eks update-kubeconfig --name <cluster-name> --region <region>
```

  ## 3. CI/CD Pipeline for Terraform
A **GitHub Actions pipeline** was configured to automate Terraform operations. The workflow:  
1. On **push to a feature branch**:  
   - Runs `terraform fmt` to enforce code formatting  
   - Runs `terraform validate` to check syntax and indentation  
   - Executes `terraform plan` to detect changes  
2. On **merge to main**:  
   - Executes `terraform apply` to provision/update infrastructure  

This ensured safe, automated infrastructure deployments while maintaining high-quality Terraform code practices. 


## Conclusion

This project successfully showcased the deployment of a containerized retail application on AWS EKS, leveraging Terraform for infrastructure automation and GitHub Actions for CI/CD.
Key accomplishments include:

- Implementation of Infrastructure as Code using a resource-specific Terraform structure
- Deployment of a secure, scalable application on Amazon EKS
- Automation of infrastructure provisioning through a GitHub Actions pipeline

### User Access

- An IAM user was provisioned in AWS and integrated with the Kubernetes cluster using ***Role-Based Access Control (RBAC)***. The user’s AWS identity was mapped to a Kubernetes role with permissions to ***read, list, and describe cluster resources***. This setup enforces secure,***least-privilege access*** while maintaining fine-grained control over cluster operations.
  
 ### User Instructions

To configure access for interacting with the Kubernetes cluster via the ***AWS CLI*** and `kubectl`, follow these steps:

- Create IAM credentials
- In the AWS Console, generate an Access Key and Secret Access Key for the user.
- Configure AWS CLI on the user's machine

Open a terminal and run the following command:

##### Update Kubeconfig for your user
```
aws eks update-kubeconfig \
  --region eu-west-2 \
  --name your-cluster-name \
  --profile dev-readonly \
  --alias dev-readonly
```
  
- --profile dev-readonly tells AWS CLI to use the new IAM user.
- --alias adds a context name so you can easily switch in kubectl

  ##### Test Permissions
  ```
  kubectl get pods

This deployment approach is production-ready and can be extended for scaling, monitoring, and further automation.  


---


