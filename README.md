# Jenkins CICD With Dynamic Application Security Testing (DAST) with OWASP ZAP


## Overview

OWASP ZAP (Zed Attack Proxy) is one of the options we have as part of the DAST (Dynamic Application Security Testing) security techniques. It identifies vulnerabilities in web applications, including compromised authentication, exposure of sensitive data, security misconfigurations, SQL injection, cross-site scripting (XSS), insecure deserialization, and components with known vulnerabilities.

## Requirements

To use this Jenkins Pipeline effectively, you need to meet the following requirements:

- **Jenkins Installation:** Ensure that Jenkins is properly installed and configured on your system.

- **Docker Installation:** Docker should be installed on the Jenkins machine. The pipeline utilizes Docker containers for various stages of the process.

  - **Docker Image:** You'll need the following Docker image installed:

    - **Use this cmd:** docker pull owasp/zap2docker-live

## Stages

### Checkout the project

- **Purpose:** This stage checks out the project's source code from the specified Git repository.

### Build the package

- **Purpose:** In this stage, the Maven tool is used to clean and build the project package.

### Test

- **Purpose:** The pipeline runs Maven tests to ensure the code is functioning correctly.

### CODE ANALYSIS WITH CHECKSTYLE

- **Purpose:** This stage performs code analysis using Checkstyle to check for code style and formatting issues.

### JUnit Tests

- **Purpose:** JUnit tests are executed in this stage, and the test results are archived for further analysis.

### Generate Jacoco Reports

- **Purpose:** Jacoco reports are generated to measure code coverage, providing insights into which parts of the code are tested.

### Building the Image

- **Purpose:** This stage builds a Docker image using the specified Dockerfile and assigns it a version number.

### Deploy the Image to Amazon ECR

- **Purpose:** The Docker image is pushed to Amazon Elastic Container Registry (ECR) for container storage and management.

### Deploy to Amazon EKS

- **Purpose:** This stage deploys the application to Amazon Elastic Kubernetes Service (EKS).
- It configures AWS credentials, updates the kubeconfig for the EKS cluster, and applies Kubernetes manifests to deploy the application.

### OWASP - Get Write Access

- **Purpose:** This step grants write permissions to the current directory for subsequent operations.

### Setting up OWASP ZAP docker container

- **Purpose:** This stage starts a Docker container named 'owasp' using the OWASP ZAP image for security testing.

### Run Application

- **Purpose:** OWASP ZAP is used to perform a security scan on the specified target URL.
- The results are saved in a report file named 'testreport.html'.

### Copy Report to Workspace

- **Purpose:** If the 'owasp' container exists, this stage copies the security scan report from the container to the Jenkins workspace.

### Stop Container

- **Purpose:** This stage stops and removes the 'owasp' container to clean up resources.

## Summary

These stages collectively define a CI/CD pipeline that automates the build, testing, security scanning, and deployment of your application. It integrates various tools and services, including Maven, Docker, Amazon ECR, and Amazon EKS, to streamline the software development and release process.
