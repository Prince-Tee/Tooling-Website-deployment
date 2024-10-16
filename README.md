# Deployment Automation with Jenkins

## Table of Contents
1. [Introduction](#introduction)
2. [Prerequisites](#prerequisites)
3. [Setting Up Jenkins](#setting-up-jenkins)
4. [Creating a New Jenkins Job](#creating-a-new-jenkins-job)
5. [Configuring Build Triggers](#configuring-build-triggers)
6. [Adding Build Steps](#adding-build-steps)
7. [Post-build Actions](#post-build-actions)
8. [Testing the Setup](#testing-the-setup)
9. [Troubleshooting](#troubleshooting)
10. [Conclusion](#conclusion)

## Introduction
This documentation provides a step-by-step guide on how to automate deployments using Jenkins. Jenkins is an open-source automation server that enables developers to build, test, and deploy software efficiently.

## Prerequisites
- A running instance of Jenkins
- Access to your source code repository (e.g., GitHub)
- Necessary plugins installed (e.g., Git plugin, SSH plugin)

## Setting Up Jenkins
1. **Install Jenkins**:
   - Follow the installation instructions for your platform from the [Jenkins website](https://www.jenkins.io/doc/book/installing/).
   
2. **Access Jenkins**:
   - Open your browser and navigate to `http://your-server-ip:8080`.

3. **Unlock Jenkins**:
   - Follow the instructions to unlock Jenkins using the initial admin password located in `/var/lib/jenkins/secrets/initialAdminPassword`.

4. **Install Suggested Plugins**:
   - During the setup, choose to install suggested plugins or select plugins that meet your requirements.

## Creating a New Jenkins Job
1. **Create New Item**:
   - On the Jenkins dashboard, click on **New Item**.

2. **Enter Job Name**:
   - Provide a name for your job and select **Freestyle project** or **Pipeline**, then click **OK**.

3. **Configure the Job**:
   - In the job configuration page, enter the necessary details such as the description and source code repository.

## Configuring Build Triggers
1. **Build Triggers Section**:
   - In the job configuration, scroll to the **Build Triggers** section.

2. **Select Trigger Options**:
   - Choose options such as:
     - **Poll SCM**: Schedule periodic checks for changes in your repository.
     - **GitHub hook trigger for GITScm polling**: Trigger builds upon commits to the repository.

## Adding Build Steps
1. **Build Steps Section**:
   - Scroll down to the **Build** section.

2. **Add Build Step**:
   - Click on **Add build step** and select the appropriate step type, such as:
     - **Execute shell**: To run shell commands.
     - **Invoke Ant**: For building Java projects.
     - **Invoke Maven**: For building Maven projects.

## Post-build Actions
1. **Post-build Actions Section**:
   - Scroll down to the **Post-build Actions** section.

2. **Select Actions**:
   - Choose actions such as:
     - **Archive the artifacts**: To store build outputs.
     - **Send build artifacts over SSH**: To transfer files to a remote server.

## Testing the Setup
1. **Save the Job Configuration**:
   - Click **Save** at the bottom of the job configuration page.

2. **Build the Job**:
   - On the job page, click **Build Now** to test the configuration.

3. **Check Console Output**:
   - Click on the build number to see the console output and verify that the build was successful.

## Troubleshooting
- **Permission Denied Errors**:
  - Ensure the Jenkins user has the appropriate permissions for the directories involved.
  
- **SSH Connection Issues**:
  - Check SSH keys and configuration settings for remote servers.

- **Build Failures**:
  - Review the console output for errors and adjust build steps accordingly.

## Conclusion
This documentation outlines the fundamental steps required to set up deployment automation using Jenkins. With proper configuration, Jenkins can significantly enhance the efficiency of your deployment processes.

For further details, consult the [Jenkins documentation](https://www.jenkins.io/doc/).
