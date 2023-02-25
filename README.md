# Final ITI DevOps-CI-CD-Project Infrastructure

## by Abdelrahman Anany

## Project Overview:
![Project Overview](https://github.com/AbdelrahmanAnany/ITI-Final-CI-CD-Project-Infrastructure/blob/main/screenshots/devops-project-overview.png)

Deploy a Python web application on GKE using CI/CD Jenkins Pipeline using the following steps and high-level diagram:
1. Implement a secure GKE Cluster
2. Deploy and configure Jenkins on GKE
3. Deploy the backend application on GKE using the Jenkins pipeline


## Tools:
| Tool | Purpose |
| ------ | ------ |
| [ Google Kubernetes Engine (GKE) ](https://cloud.google.com/kubernetes-engine) | Google Kubernetes Engine (GKE) is a managed, production-ready environment for running containerized applications. |
| [ Jenkins ](https://www.jenkins.io) | Jenkins – an open-source automation server is enabling developers worldwide to reliably build, test, and deploy their software. |
| [ Helm ](https://helm.sh) | Helm helps you manage Kubernetes applications — Helm Charts help you define, install, and upgrade even the most complex Kubernetes applications. |
| [ Docker ](https://www.docker.com) | Docker is a set of platform-as-a-service (PaaS) products that use OS-level virtualization to deliver software in containers|
| [ Terraform ](https://www.terraform.io) | Terraform is an open-source infrastructure as a code software tool that enables you to safely and predictably create, change, and improve infrastructure. |


## First Part: Infrastructure Overview

- ###  Network Files Consist of :
  - Two subnets one for GKE and another for Bastion Host
  - NAT Gateway 
  - Firewall to allow SSH Connection

- ### GKE Files Consist of:
  - private container cluster resource with authorized networks configuration
  - node pool with count 3 
- ### Bastion File: 
    - for Creating a Private VM to Connect with GKE Cluster

## Second Part: Build the Infrastructure
### 1. Clone The Repo:
```
git clone https://github.com/AbdelrahmanAnany/ITI-Final-CI-CD-Project-Infrastructure/
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
## Third Part: Connect to Private GKE Cluster through Bastion VM
> After the Infrastructure is built navigate to `Compute Engine` from the GCP console then `VM instances` and click the SSH to `private-instance` to run these commands:
![vm-instance](https://github.com/AbdelrahmanAnany/ITI-Final-CI-CD-Project-Infrastructure/blob/main/screenshots/vm-instance.png)

### 1. Install Kubectl
```
sudo apt-get install kubectl
```
### 2. Install GKE gcloud auth Plugin
```
sudo apt-get install google-cloud-sdk-gke-gcloud-auth-plugin
```
### 3. Log in with your Credentials
```
gcloud auth login
```
### 4. Set your active Application Default Credentials
> to set your active Application Default Credentials to your account run these commands:
```
gcloud auth application-default login
```
### 5. Connect to GKE Cluster
> Go to the `Kubernetes Engine` Page in your `Clusters` tab you will find the `private-cluster`

![private-cluster](https://github.com/AbdelrahmanAnany/ITI-Final-CI-CD-Project-Infrastructure/blob/main/screenshots/private-cluster.png)

> Click on the `Action button` "Three dots" then `Connect`, Copy the command and paste it into the `VM SSH window`
```
gcloud container clusters get-credentials private-cluster --zone us-central1-a --project iti-abdelrahman
```
### 6. Building the Dockerfile for jenkins and pushing to Dockerhub
Create the Dockerfile:
Then:

    docker build -t 3anany/jenkinsgcp
    docker push 3anany/jenkinsgcp

![](https://github.com/AbdelrahmanAnany/ITI-Final-CI-CD-Project-Infrastructure/blob/main/screenshots/jenkins-image.png)

Deploying the jenkins app
Create the deployment.yml file

```
    kubectl apply -f deployment.yml
```
### 7. Get `admin` user Password

Connect to Cluster via VM and type
```
  kubectl exec --namespace jenkins -it svc/jenkins-service -c jenkins -- /bin/cat /var/jenkins_home/secrets/initialAdminPassword && echo
```
### 8. Get the `Jenkins URL`
```
kubectl get all -n jenkins
```
and copy the external IP address and paste in browser url as $ExternalIP:port
![](https://github.com/AbdelrahmanAnany/ITI-Final-CI-CD-Project-Infrastructure/blob/main/screenshots/jenkins.png)


**Connected Repository**

Jekins CI/CD Repository: https://github.com/AbdelrahmanAnany/ITI-Final-CI-CD-Project-Application
