# tech-challenge-1-ecs-jenkins
Node.js frontend + backend deployed to ECS Fargate with Terraform and Jenkins CI/CD
Tech Challenge 1 — Node.js Frontend & Backend on AWS ECS (Fargate)
Overview

This project demonstrates a complete end-to-end DevOps workflow for deploying a containerized React frontend and Node.js (Express) backend to AWS ECS using Fargate, with CI/CD automation via Jenkins and infrastructure provisioned using Terraform.

The frontend communicates with the backend over HTTP, and all deployments are fully automated through a Jenkins pipeline that builds Docker images, pushes them to Amazon ECR, and updates ECS services.

Architecture Summary

Frontend: React (Dockerized)

Backend: Node.js / Express (Dockerized)

Container Orchestration: AWS ECS (Fargate)

CI/CD: Jenkins

Infrastructure as Code: Terraform

Container Registry: Amazon ECR

Networking: Application Load Balancer (public access)

Auto Scaling: ECS Service Auto Scaling based on CPU utilization

Repository Structure
.
├── frontend/
│   ├── Dockerfile
│   ├── src/
│   │   └── config.js
│   └── package.json
├── backend/
│   ├── Dockerfile
│   ├── config.js
│   └── package.json
├── rules.json
├── td-backend.json
├── td-backend-register.json
├── frontend/td-frontend.json
├── frontend/td-frontend-register.json
├── frontend/updated-task-def.json
└── README.md

Local Development
Prerequisites

Node.js 16+

Docker

npm

Run Backend Locally
cd backend
npm ci
npm start


Backend will be available at:

http://localhost:8080

Run Frontend Locally
cd frontend
npm ci
npm start


Frontend will be available at:

http://localhost:3000


If configured correctly, the frontend will display “SUCCESS” followed by a GUID.

Dockerization
Build Backend Image
cd backend
docker build -t tc1-backend .

Build Frontend Image
cd frontend
docker build -t tc1-frontend .


Both services are deployed to AWS as containers via ECS Fargate.

AWS Deployment Overview
ECS Configuration

Launch Type: Fargate

CPU per task: 512 (0.5 vCPU)

Memory per task: 1024 MB (1 GB)

Desired tasks: 1

Min tasks: 1

Max tasks: 4

Auto Scaling Trigger: 50% CPU utilization

Networking

Public Application Load Balancer

Frontend exposed publicly

Backend accessible via ALB target group

CI/CD Pipeline (Jenkins)

The Jenkins pipeline performs the following steps:

Pulls source code from GitHub

Builds Docker images for frontend and backend

Pushes images to Amazon ECR

Registers updated ECS task definitions

Updates ECS services to deploy new containers

This enables fully automated deployments on every pipeline run.

Jenkins Infrastructure

The Jenkins server was deployed manually on AWS (EC2) and includes:

IAM role with permissions for ECS, ECR, and CloudWatch

Public access over HTTP

Jenkins credentials configured for AWS access

## Live Application URLs

Frontend URL:
http://tc1-alb-1207929775.us-east-2.elb.amazonaws.com

Backend API (via ALB):
/api
/api/health
/api/status

The frontend and backend are deployed behind a single Application Load Balancer using
path-based routing. The backend responds successfully to API requests. The frontend
loading state reflects an application-level response handling mismatch rather than
an infrastructure issue.

Key DevOps Concepts Demonstrated

Infrastructure as Code (Terraform)

Containerization with Docker

ECS Fargate orchestration

CI/CD automation with Jenkins

Blue/green-style service updates

Auto scaling based on metrics

Secure AWS IAM role usage

Interview Talking Points

Why Fargate was chosen over EC2-based ECS

How CI/CD reduces deployment risk

How task definitions enable versioned deployments

How auto scaling improves reliability

How Terraform ensures repeatable infrastructure
