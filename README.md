This repository implements a robust, automated CI/CD pipeline using GitHub Actions, Maven, Trivy, and Amazon ECR. It ensures every change is built, tested, scanned for security vulnerabilities, and deployed as a containerized image to AWS.
🛠 Workflow Overview
The pipeline, named "Build Job," is triggered on:
Push & Pull Requests: Targeting the main branch.
Manual Trigger: via workflow_dispatch.
Scheduled: Every Sunday at midnight (UTC) for routine health checks.
🏗 Pipeline Stages (Jobs)
Build
Checks out the code using actions/checkout.
Compiles the Java application using Maven (mvn install).
Uploads the resulting .war file as a GitHub artifact named vprofile-app.
Testing
Runs unit tests (mvn test) and static code analysis (mvn checkstyle:checkstyle).
Note: Testing logic is strictly enforced for the main branch to maintain stability.
Security_scan
Performs a filesystem vulnerability scan using the Aquasecurity Trivy Action.
Generates a trivy-results.json report and uploads it as an artifact for audit compliance.
BUILD_AND_PUBLISH (Deployment)
Environment: Production.
Auth: Securely connects to AWS via OIDC/Secrets.
Action: Builds a multi-stage Docker image, tags it with the unique github.sha, and pushes it to your Amazon ECR repository (pa1sai-appimage).
