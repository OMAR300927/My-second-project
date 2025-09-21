My second demo project: CI/CD pipeline for deploying static HTML page

Build and deploy a simple HTML page using Jenkins, Kubernetes and Ansible

# Prerequisites

* HTML page
* Docker
* Git and GitHub
* Jenkins
* Kubernetes
* Ansible

# Steps in the CI/CD pipeline

1. Create a "This is my second project" HTML page
2. Dockerize the HTML page
3. Create a GitHub repository and push code to it
4. Start Jenkins server on a host
5. Using ngrok to get an IP for Jenkins and use webhook trigger with GitHub
6. Write Jenkins pipeline to build and push the Docker image to Docker
7. Set up Kubernetes on a host using Minikube
8. Create a Kubernetes deployment and service for the HTML page
9. Create a Kubernetes deployment for mongoDB as DB for the HTML page ( It's just training add-on )
10. Create a Kubernetes Nginx deployment to work as reverse proxy and expose the service externally
11. Write an Ansible playbook to deploy Kubernetes resources
12. Use Jenkins to connect to the VM and use the playbook to deploy the Kubernetes cluster

# Project structure

| File                  | Description
|-----------------------|---------------------------------------------------------------------------------------------|
| index.html            |  HTML page which will print "This is my second project" when you run it                     |
| Dockerfile            |  Contains commands to build and run the Docker image                                        |
| Jenkinsfile           |  Contains the pipeline script for build , push and deploy                                   |
| html.yaml             |  Kubernetes deployment and service file for the HTML page                                   |
| html-secret.yaml      |  Kubernetes secret file to put username and password to the page                            |
| nginx-configmap.yaml  |  Kubernetes configmap file where i put the default.conf                                     |
| mongoDB.yaml          |  Kubernetes deployment and service file for mongoDB                                         |
| nginx.yaml            |  Kubernetes nginx file that works as reverse proxy and expose HTML page service externally  |
| apply-cluster.yml     |  Ansible file on the VM to deploy the Kubernetes resources                                  |

# Conclusion

* In this project, I built a very simple CI/CD pipeline using Jenkins, Kubernetes and Ansible
* Don't forget to use ngrok in terminal and keep the terminal open so you can use the webhook .
* I used credentials for Docker Hub, after deploy the application, run `minikube service html-service` to access it in browser
* Or you can use the tunnel by run `minikube tunnel` to access it , But if you use this method, you should also update your hosts file

# Notes

* I used inventory.ini file on VM
* And use ansible-vault by creating an encrypted vault.yml with vault_key.yml to decrypt it on the VM
* I used -tt to force activation TTY , and -vvv for debugging
* This is just static HTML page so the html-secret will not work , you need to use something like ingress or backend app like flask or node.js
