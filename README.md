🚀 CI/CD Pipeline for pa1sai-appimage

This repository uses GitHub Actions to automate the build, test, security scan, and deployment of a Java-based application to Amazon Elastic Container Registry (ECR). Everything runs securely and efficiently—ensuring high-quality, production-ready container images.

🔧 What This Pipeline Does

✅ Build

Triggered on every push to main, pull requests to main, manual runs, or weekly (Sundays at midnight).
Compiles the project with Maven (mvn install).
Uploads the resulting .war file as a build artifact.
🧪 Testing

Runs unit tests and Checkstyle validation only on the main branch.
Ensures code quality before proceeding to deployment.
🛡️ Security Scan

Uses Trivy to scan the codebase for OS and library vulnerabilities.
Saves detailed scan results as a downloadable artifact (trivy-results.json).
🐳 Build & Publish to ECR

Builds a Docker image using a multi-stage Dockerfile for minimal size.
Pushes the image to Amazon ECR tagged with the Git commit SHA:
<your-ecr-registry>/pa1sai-appimage:<commit-sha>
Only runs on the main branch and after all prior jobs succeed.
Uses GitHub Environments for production safety.
🌐 Tech Stack


Component
Tool
Build
Apache Maven
CI/CD
GitHub Actions
Containerization
Docker (Multi-stage)
Registry
Amazon ECR
Security
Trivy (by Aqua Security)
Cloud
AWS
🛠️ How to Use This Setup in Your Project

Want to replicate this pipeline? Follow these steps:

1. Prerequisites

A Maven-based Java project that produces a .war file.
A Dockerfile (e.g., in Docker-files/app/multistage/Dockerfile).
An AWS account with ECR access.
2. Set Up GitHub Secrets & Variables

Go to Settings > Secrets and variables > Actions and add:

Secrets:
AWS_ACCESS_KEY_ID
AWS_SECRET_ACCESS_KEY
Variables:
AWS_REGION (e.g., us-east-1)
🔒 Best Practice: Use an IAM user with least-privilege permissions for ECR push.
3. Create ECR Repository

bash


1
aws ecr create-repository --repository-name pa1sai-appimage --region YOUR_REGION

4. Add the Workflow

Save the workflow YAML as .github/workflows/ci-cd.yml in your repo.

5. Customize Paths

Adjust the Dockerfile path if needed.
Ensure your Maven build outputs to target/*.war.
6. Optional Enhancements

Enable testing on all branches (update conditionals).
Replace AWS keys with OIDC federation for better security.
Add notifications (Slack, email) on failure.
