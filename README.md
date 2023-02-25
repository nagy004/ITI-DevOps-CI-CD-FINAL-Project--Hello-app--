# Final ITI DevOps-CI-CD-Project Infrastructure

## by Abdelrahman Anany

## Project Overview:

Deploy a Python web application on EKS using CI/CD Jenkins Pipeline using the following steps:
1. Implement a secure EKS Cluster
2. Deploy and configure Jenkins on EKS
3. Deploy the backend application on EKS using the Jenkins pipeline


## Tools:
| Tool | Purpose |
| ------ | ------ |
| [ Amazon (EKS) ](https://aws.amazon.com/solutions/implementations/amazon-eks/) | Amazon Elastic Kubernetes Service (EKS) is a managed service and certified Kubernetes conformant to run Kubernetes on AWS. |
| [ Jenkins ](https://www.jenkins.io) | Jenkins – an open-source automation server is enabling developers worldwide to reliably build, test, and deploy their software. |
| [ Docker ](https://www.docker.com) | Docker is a set of platform-as-a-service (PaaS) products that use OS-level virtualization to deliver software in containers|
| [ Terraform ](https://www.terraform.io) | Terraform is an open-source infrastructure as a code software tool that enables you to safely and predictably create, change, and improve infrastructure. |


## First Part: Infrastructure Overview

- ###  Network Files Consist of :
  - Two subnets one for GKE and another for Bastion Host
  - NAT Gateway 
  - Firewall to allow SSH Connection

- ### GKE Files Consist of:
  - private container cluster resource 
  - node group with count 1
- ### Bastion File: 
    - for Creating a Private VM to Connect with EKS Cluster Node 

## Second Part: Build the Infrastructure
### 1. Clone The Repo:
```
git clone https://github.com/nagy004/Final-ITI-DevOps-CI-CD-Project-Infrastructure
```
### 2. Navigate to Terraform Code
> After you clone the code navigate to the `terraform` folder to build the infrastructure:
```
cd terraform/
```
#### 3. Initialize Terraform
```
terraform init
```

#### 4. Check Plan
```
terraform plan
```

#### 5. Apply the plan
```
terraform apply
```
## Third Part: Connect to Private EKS Cluster Node through Bastion VM 
> After You ssh on the private Node install Docker on it by applying the following steps :

### 1. Update your system's package index by running the following command:
 
```
sudo yum update
```
### 2. Install the required packages to set up the Docker repository:

```
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```
### 3. Use the following command to set up the Docker repository:

```
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

```
### 4. Install Docker by running the following command:

```
sudo yum install docker-ce docker-ce-cli containerd.io

```
### 5. Start and enable Docker to automatically start at boot time:

```
sudo systemctl start docker
sudo systemctl enable docker
```
### 6. Verify that Docker is installed and running by running the following command:


```
   sudo docker run hello-world

```
### 7. connect with the EKS cluster API 

```
aws eks --region us-east-1 update-kubeconfig --name nagy-eks

```
### 7. make your name spaces 

```
kubectl create namespace <namespace-name>

```

### 8. get into the cluster directory and use the yaml files in it to make your cluster deployment & services  
```
kubectl get apply -f <file-name>
```

### 9. get your working pods and services
```
kubectl get pods -n <namespace-name>
kubectl get svc -n <namespace-name>

```
### 10. complete the installation for you jenkins and make your managed credintials
### 11. Run your pipe-line 



**Connected Repository**
https://github.com/nagy004/Final-ITI-DevOps-CI-CD-Project-Infrastructure

