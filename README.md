# Kubernetes Deployment with Docker & GitHub Actions

This repository automates the process of building Docker images, pushing them to GitHub Container Registry (GHCR), and deploying them to a Kubernetes cluster using GitHub Actions and Ansible. Below is a guide to get started and understand the setup.

## Repository Structure

.
├── Dockerfile                # Dockerfile for building the image
├── index.html                # Web page served by the Nginx container
├── nginx-deployment.yml      # Kubernetes deployment definition for Nginx
├── nginx-service.yml         # Kubernetes service definition for Nginx
├── inventory                 # Ansible inventory folder
│   └── hosts.ini             # Inventory file for local hosts
├── .github
│   └── workflows
│       └── docker-image-build-pipeline.yml  # GitHub Actions workflow for CI/CD
└── deploy-playbook.yml       # Ansible playbook for Kubernetes deployment

## Prerequisites
Ensure you have the following tools installed:

Docker Desktop: To build and push Docker images locally and interact with Docker containers.
kubectl: For interacting with Kubernetes clusters.
minikube: For local Kubernetes clusters.
Ansible: For Kubernetes deployment automation.
Python 3: Required for the Kubernetes Python client.

Missing Dependencies: If any dependencies are missing, the playbook's debug messages will notify you with clear instructions on what needs to be installed.
Deployment Issues: If deployment fails, you can check pod logs using kubectl logs <pod-name> or view the deployment status with kubectl get pods.

## How it works

# GitHub Actions Workflow
This GitHub Actions pipeline builds and pushes a Docker image to GitHub Container Registry (GHCR) whenever a version tag (e.g., v1.0.0) is pushed to the repository.

Key steps:
Checkout code: Retrieves the latest code from the repository.
Lint Dockerfile: Runs a linter to check the Dockerfile for potential issues.
Set environment variables: Configures necessary variables like repository name, image name, and tag, and sets up the Kubernetes config.
Check dependencies: Verifies that Docker, kubectl, minikube, and Ansible are installed.
Log in to GHCR: Authenticates to the GitHub Container Registry.
Build Docker image: Builds the Docker image with a dynamic tag based on the version.
Push Docker image: Pushes the image to GHCR for use in Kubernetes deployment.

# Ansible playbook

This Ansible playbook deploys an Nginx app to a local Kubernetes cluster (Minikube):
Check Dependencies: Verifies if Python, pip3, Ansible, kubectl, minikube, and the Kubernetes Python client are installed, with debug messages if any are missing.
Set Installation Packages: Determines the correct kubectl and minikube packages based on the OS.
Fetch Versions: Retrieves the current deployment's image tag (incase new image version doesn't work and a rollback is necesary) and fetches the latest GitHub tag for the new image version.
Deploy: Updates the Kubernetes deployment and service with the new image tag.
Verify: Checks the pod status to ensure deployment success.
Rollback: Rolls back to the previous version if needed.
This playbook automates deployment while ensuring dependencies are met and using the latest image tag from GitHub.


