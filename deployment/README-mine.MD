Project README

This project is a coworking analytics application deployed using Kubernetes on AWS Elastic Kubernetes Service (EKS). The application container is built using Docker, connected to a local database managed by Kubernetes, and automated through AWS CodeBuild.

Prerequisites

AWS account with permissions to create EKS clusters, ECR repositories, and CodeBuild projects.
Docker installed locally.
kubectl installed and configured to interact with your EKS cluster.

Docker Image

Build the Docker Image:
	docker build -t test-coworking-analytics .

Run the Docker Container:
docker run --network="host" test-coworking-analytics


Kubernetes Configuration

ConfigMap:

Create a configmap.yaml file containing database configuration.
Apply the ConfigMap
	kubectl apply -f configmap.yaml

Secret:

Store the database password in a Kubernetes Secret.
Apply the Secret
	kubectl apply -f secret.yaml

Deployment:

Create a coworking.yaml file for the deployment script.
Apply the Deployment:
	kubectl apply -f coworking.yaml


AWS CodeBuild Integration
CodeBuild Project:
Set up a CodeBuild project in AWS.
	Our buildspec is in the root folder of this repo.

By following these steps, you can successfully deploy the coworking analytics application using Kubernetes on AWS EKS.

