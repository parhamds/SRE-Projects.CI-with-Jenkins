# Project Overview: CI with Jenkins

This project aims to establish a Continuous Integration (CI) pipeline using Jenkins for automating the build, analysis, and deployment processes of a software project. The pipeline is designed to trigger upon code changes pushed to a GitHub repository, utilizing various tools and services such as SonarQube for code analysis, Nexus for artifact management, and Slack for notifications.

## Overview:

Developers push code changes to a GitHub repository, which triggers a Jenkins job via a webhook. The Jenkins job orchestrates the following processes:
- Building the code
- Analyzing the code using SonarQube
- Uploading artifacts to Nexus
- Sending notifications to a Slack channel

## Setup Steps:

1. **Create Key Pair and Security Groups (SGs):**
    - Generate a `.pem` keypair for EC2 instance access.
    - Configure security groups:
        - Jenkins SG: Allow port 22 from developer IPs, port 8080 from any IP, and access to SonarQube SG.
        - Nexus SG: Allow port 22 from developer IPs, port 8081 from Jenkins SG, and any IP.
        - Sonar SG: Allow port 22 from developer IPs, port 80 from Jenkins SG, and developer IPs.

2. **Provision EC2 Instances:**
    - Set up EC2 instances for Jenkins, SonarQube, and Nexus.
    - Install required software using user data.

3. **Configure Nexus:**
    - Connect to Nexus on port 8081, create repositories:
        - Maven2 hosted (vprofile-release) for storing artifacts.
        - Maven2 proxy (vpro-maven-central) for managing dependencies.
        - Maven2 hosted (vprofile-snapshot) for storing snapshots (optional).
        - Maven2 group (vpro-maven-group) to group repositories.

4. **Connect to SonarQube:**
    - Create a quality gate to measure code quality.
    - Set up a webhook to send analysis results to Jenkins.

5. **Set Up Slack Integration:**
    - Create a Slack account and workspace.
    - Add a channel for notifications.
    - Integrate Jenkins with Slack by copying the generated token.

6. **Jenkins Configuration:**
    - Install necessary plugins: Maven integration, GitHub integration, Nexus artifact uploader, SonarQube scanner, Slack notification, Build timestamp.
    - Configure tools: SonarQube scanner, Maven, JDK.
    - Configure system settings: Sonar scanner details, build timestamp pattern, Slack integration details.
    - Add Nexus credentials to Jenkins.

7. **Create Jenkins Pipeline:**
    - Define stages for the pipeline:
        - Install Maven and set up Nexus as a repository for dependencies.
        - Build the code, upload artifacts, and test the application.
        - Perform Sonar analysis and wait for the response.
        - Verify quality gate status.
        - Upload artifacts to Nexus.
        - Send Slack notifications.

8. **Configure Jenkins Job:**
    - Create a Jenkins job based on the pipeline.
    - Add GitHub repository details, credentials, and webhook.

## Usage:

1. Developers push code changes to the linked GitHub repository.
2. Jenkins automatically triggers the CI pipeline.
3. The pipeline builds, tests, analyzes, and deploys the code.
4. Notifications are sent to the configured Slack channel.
