# PingDev

Pre-Requisites

Create a Ping Identity account https://www.pingidentity.com/en/account/register.html
Install Docker for Mac, Linux, or Windows from https://docs.docker.com/get-docker/
If you're using Windows ensure that you're using Windows Subsystem for Linux version 2 https://docs.microsoft.com/en-us/windows/wsl/install-win10
You will need to create a Docker account if you do not have one. Install Docker, then launch the program and select preferences (menu bar icon to open ). Open the Resources preferences.
Under General, uncheck 'Start Docker Desktop when you log in' so that you can control when it runs
Under Advanced set memory to 16 GB, 4 CPUs, and a 1GB swap
Apply the changes and restart docker
Close the preferences
Install Homebrew on a Mac or Linux or use an alternative package manager (https://alternativeto.net/software/homebrew/)
Download and instructions here: https://brew.sh/. Install the required software from Homebrew.
Install kubectl:
brew install kubectl
Install kustomize:
brew install kustomize
Request a GTE DevOps account if you don't have one already https://docs.google.com/forms/d/e/1FAIpQLSdgEFvqQQNwlsxlT6SaraeDMBoKFjkJVCyMvGPVPKcrzT3yHA/viewform
Create a GitHub account if you don't have one already https://github.com/join
Install Visual Studio Code. Download from: https://code.visualstudio.com/download. Install it. Run it. Install in Visual Studio Code, extensions:
Docker - the one from Microsoft
YAML - this one is from Red Hat
Kubernetes
Project Manager
GitLens
Environment setup for GTE:
Open the Docker preferences again
Select the Kubernetes tab
Check 'Enable Kubernetes'
Check 'Show system containers'
Uncheck 'Deploy Docker Stacks in Kubernetes by default'
Apply the changes and restart docker
Close the preferences
Download the PingFederate components
PingFederate MSI Install (for Windows): https://drive.google.com/file/d/1foeH-MdWn54zVTrzM8ryKv0wLDRTmuTC/view?usp=sharing
PingFederate Zip Install (for Linux/Mac): https://drive.google.com/file/d/1qz4qxlwURb2nN4lv5-kuzWIgZwIJK2xa/view?usp=sharing
Agentless Integration Kit: https://drive.google.com/file/d/1SJQ4GHsoRHYIZTVBeJm0GBIkGwTlRcU-/view?usp=sharing
Verify that the pre-requisites were installed correctly by executing the following commands:
Set the $PING_IDENTITY_DEVOPS_USER and $PING_IDENTITY_DEVOPS_KEY values with the literal values that you were provided by the GTE team by executing:
kubectl create secret generic devops-secret --from-literal=PING_IDENTITY_DEVOPS_USER=$PING_IDENTITY_DEVOPS_USER --from-literal=PING_IDENTITY_DEVOPS_KEY=$PING_IDENTITY_DEVOPS_KEY
kubectx - Docker or docker-desktop should be highlighted
kubens - To list namespaces, default should be highlighted
If default is not highlighted execute:
kubens default
Thanks for your efforts in completing the required setup
Instructions

Start Docker Desktop, if not already running
Verify that docker Kubernetes is running. If you have trouble check your local host file and try restarting Docker/Kubernetes in the Troubleshooting settings
Start Visual Studio Code, create a workspace where you will save your files
First Mt Fuji Assignment: Running PingFederate using Kubernetes

Let's push the button and see the magic? The objective of this lab is to do your first deployment to your Kubernetes cluster that is running on your laptop using Docker.
kustomize build https://github.com/pingidentity/pingidentity-devops-getting-started/20-kubernetes/01-standalone/pingfederate >fuji1-pf.yaml 
This will build a YAML file for you to deploy.

Execute the following commands, in order
kubectl apply -f fuji1-pf.yaml
kubectl logs -f <your pod name here>
Execute the following command:
kubectl port-forward service/pingfederate 9888:9999
Open a browser to https://localhost:9888/pingfederate/app
Accept certificate exception if prompted
Login (password after the /) as: administrator/2FederateM0re
Second Mt Fuji Assignment: Running PIngFederate using Kustomize

Create a new lab space and folder in Visual Studio
In that folder execute:
touch kustomization.yaml
Add these contents to that new file:
kind: Kustomization
apiVersion: kustomize.config.k8s.io/v1beta1

resources:
- github.com/pingidentity/pingidentity-devops-getting-started/20-kubernetes/01-standalone/pingfederate

Execute:
kustomize build . >fuji2-deployment.yaml
kubectl apply -f fuji2-deployment.yaml
Execute the following command:
kubectl port-forward service/pingfederate 9888:9999
Open a browser to https://localhost:9888/pingfederate/app
Accept the certificate if prompted
Login as: administrator/2FederateM0re
Bonus Points

Note that the system requirements for this bonus are higher than they were for parts 1 and 2. If necessary you can tune the PingDirectory product so that it will start.
For bonus points set up Server Profiles to demonstrate using application configuration as source code.
Create a new lab space and folder in Visual Studio
Clone the repo to this new folder:
git clone https://github.com/pingidentity/pingidentity-server-profiles.git
Copy over the YAML from fuji2-deployment.yaml to fuji3-deployment.yaml as a starting point
Create a kustomization.yaml file with one that you download from: https://drive.google.com/file/d/1dNMxk_3o1LaTxKl17XidIeqdV916Lr9A/view?usp=sharing
In this kustomization.yaml file add profile-volumes.yaml as a resource. You can download this file from: https://drive.google.com/file/d/1fCxPddwFjXh5ZUXmKA4qbOwQKKDV_kb7/view?usp=sharing
Execute:
kustomize build . >fuji3-local-profile-deploy.yaml
kubectl apply -f fuji3-local-profile-deploy.yaml
Change kustomization.yaml to use a different server profile:
Replace <path to your user home folder>/lab03/pingidentity-server-profiles/getting-started/pingfederate
With <path to your user home folder>/lab03/pingidentity-server-profiles/baseline/pingfederate
And replace <path to your user home folder>/lab03/pingidentity-server-profiles/getting-started/pingdirectory
With <path to your user home folder>/lab03/pingidentity-server-profiles/baseline/pingdirectory
Execute the following:
kubectl delete -f fuji3-local-profile-deploy.yaml
kustomize build . >fuji3-local-profile-deploy.yaml
kubectl apply -f fuji3-local-profile-deploy.yaml
Login to PingFederate and take a look. Notice that there is now a baseline configuration deployed.
Execute the following command: 
kubectl port-forward service/pingfederate 9888:9999
Open a browser to https://localhost:9888/pingfederate/app
Accept the certificate if prompted
Login as: administrator/2FederateM0re
