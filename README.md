# Deploy-to-Azure-Kubernetes-from-Azure-DevOps
The goal of this project is to deploy to AKS from Azure DevOps. The AKS will pull the image I uploaded to the Azure Container Registry (Private).

In this lab, I created a Pipeline that build docker images, push them to Azure Container Registry and Publish Kubernetes Manifests to Azure Pipelines and then deploy to Azure Kubernetes Service using Azure DevOps.






Steps:

	1. For my k8s deployment, I created a minimal docker image using Dockerfile, deployment, and service yaml manifest.
	
	2. The folder was then uploaded to the GitHub repository I'd set up for this lab.
	
	       git push origin master
	
	3. I created an Azure container registry with the specifications I wanted and an Azure Kubernetes Service cluster after logging in to my Azure subscription via the command line using the command below.
	
	      az login      
	      az acr create -n MY-ACR -g iar-iwag-RG ---sku basic
	
	4. Additionally, I created an Azure Kubernetes service cluster that suited my requirements using the command below
	   
	      az aks create -n myAKSCluster2022 -g iar-iwag-RG --generate-ssh-keys --attach-acr $MY-ACR
	
	5. From Azure DevOps, I created a project under my organisation.

    6. Under the project settings created, I configured service connection to my GitHub repo and Azure resource     manager where I connected to my k8s cluster and the ACR.

    The steps in my multi-stage yaml process are listed below.

1. Building a Docker image and pushing it to the Azure Container Registry
2. Copy files (from the Kube-manifests folder) from the system's default working directory to the build artifact directory.
3. Publish build artifacts to Azure Pipelines so that release pipelines can use them. Afterward, download it during the deployment phase.
4. In order to pull my image for the deployment, I created a secret that I used to authenticate in to the Azure Container Registry (ACR).
5. Use the deployment files in the manifests to deploy to Azure Kubernetes Service.




The environment I set up for the task makes this deployment beautiful because it enables me to access my workload from the Azure DevOps workspace without having to log in to the Azure portal. This enable traceability, deployment history, diagnose resource health and also, when you target an environment from your pipelines you don’t have to specify the service connection for your resources, because pipelines automatically use the service connection defined in that environment.



