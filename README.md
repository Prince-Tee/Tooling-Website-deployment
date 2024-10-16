# Tooling Website deployment automation with Continuous Integration. Introduction to Jenkins

## 0. Launch an Ubuntu EC2 Instance

1. **Create Security Group for Jenkins**  
   - Launch a t3.medium Ubuntu 24.04 LTS instance on AWS named `jenkins-server` with the security group `jenkins-server-sg` (allow inbound traffic on port 8080 for Jenkins, and port 22 for SSH access).
   - This setup isolates the Jenkins instance for better security and management.

   ![screenshot](https://github.com/Prince-Tee/Tooling-Website-deployment/blob/main/screenshot%20from%20my%20local%20en/create%20the%20jenkins%20server.PNG)

   ![screenshot](https://github.com/Prince-Tee/Tooling-Website-deployment/blob/main/screenshot%20from%20my%20local%20en/creating%20the%20jenkins%20server%20with%20port%2022%20and%208080.PNG)

2. **Login to the Instance via SSH** 

   ![ssh into the jenkins intance](https://github.com/Prince-Tee/Tooling-Website-deployment/blob/main/screenshot%20from%20my%20local%20en/ssh%20into%20the%20jenkins%20instance.PNG)

   Use SSH or EC2 instance connect:
   ```bash
   sudo apt update -y && sudo apt upgrade -y
   ```
     ![screenshot](https://github.com/Prince-Tee/Tooling-Website-deployment/blob/main/screenshot%20from%20my%20local%20en/updating%20ubuntu.PNG)

## 1. Install and Configure Jenkins

### Step 1: Install JDK  
Jenkins requires Java, so install OpenJDK:
```bash
sudo apt update
sudo apt install fontconfig openjdk-17-jre
java -version
```
![screenshot](https://github.com/Prince-Tee/Tooling-Website-deployment/blob/main/screenshot%20from%20my%20local%20en/installs%20OpenJDK%20version.PNG)

### Step 2: Install Jenkins  
1. Add the Jenkins repository key and update sources:
   ```bash
   sudo wget -O /usr/share/keyrings/jenkins-keyring.asc https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
   echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/" | sudo tee /etc/apt/sources.list.d/jenkins.list
   sudo apt-get update
   ```
    ![screenshot](https://github.com/Prince-Tee/Tooling-Website-deployment/blob/main/screenshot%20from%20my%20local%20en/install%20jenkins.PNG)

2. Install Jenkins:
   ```bash
   sudo apt-get install jenkins
   ```
   ![screenshot](https://github.com/Prince-Tee/Tooling-Website-deployment/blob/main/screenshot%20from%20my%20local%20en/install%20jenkins.PNG)

3. Verify Jenkins is running:
   ```bash
   sudo systemctl status jenkins
   ```
  ! [screenshot](https://github.com/Prince-Tee/Tooling-Website-deployment/blob/main/screenshot%20from%20my%20local%20en/jenkins%20runing%20sudo%20systemctl%20status%20jenkins.PNG)

4. Access Jenkins via browser using its public IP and port `8080`:
   ```
   http://<jenkins-server-public-ip>:8080
   ```
   ![screenshot](https://github.com/Prince-Tee/Tooling-Website-deployment/blob/main/screenshot%20from%20my%20local%20en/Jenkins%20from%20the%20web%20broswer%208080.PNG)

### Step 3: Unlock Jenkins  
Retrieve the initial administrator password by running the below command and copy the strings of letters/numbers:
```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
![screenshot](https://github.com/Prince-Tee/Tooling-Website-deployment/blob/main/screenshot%20from%20my%20local%20en/jenkins%20password.PNG)
### Step 4: Setup Jenkins  
1. Install the suggested plugins.

![screenshot](https://github.com/Prince-Tee/Tooling-Website-deployment/blob/main/screenshot%20from%20my%20local%20en/after%20password%20it%20will%20take%20you%20to%20this%20page.PNG)

![screenshot](https://github.com/Prince-Tee/Tooling-Website-deployment/blob/main/screenshot%20from%20my%20local%20en/installing%20the%20suggested%20plugins.PNG)

2. Create the admin user (`<your password>` for test environments).
3. Complete setup by accepting the root URL.
     
![screenshot](https://github.com/Prince-Tee/Tooling-Website-deployment/blob/main/screenshot%20from%20my%20local%20en/go%20to%20the%20jenkins%20console.PNG)

![screenshot](https://github.com/Prince-Tee/Tooling-Website-deployment/blob/main/screenshot%20from%20my%20local%20en/save%20and%20finish%20jenkins%20on%20webbroswer.PNG)

## 2. Configure Jenkins to Retrieve Code from GitHub Using Webhooks

### Step 1: Enable GitHub Webhooks
1. Go to your GitHub repository's **Settings** > **Webhooks**.

![screenshot](https://github.com/Prince-Tee/Tooling-Website-deployment/blob/main/screenshot%20from%20my%20local%20en/select%20webhook%20at%20the%20left%20corner.PNG)

2. Add a webhook with the URL `http://<jenkins_server_ip>:8080/github-webhook/`.
3. Select **application/json** for the content type and check **Push events**.
4. Activate the webhook.

![screenshot](https://github.com/Prince-Tee/Tooling-Website-deployment/blob/main/screenshot%20from%20my%20local%20en/after%20clicking%20add%20we%20configured%20the%20webhook.PNG)

### Step 2: Create Jenkins Job
1. On Jenkins, click **New Item** > **Freestyle Project**, name it `toolingjob_github`.
2. Configure the project to connect to the GitHub repository and provide credentials.

![screenshot](https://github.com/Prince-Tee/Tooling-Website-deployment/blob/main/screenshot%20from%20my%20local%20en/github%20url%20user%20and%20password.PNG)

3. Save the configuration and manually run the build via **Build Now**.

![screenshot](https://github.com/Prince-Tee/Tooling-Website-deployment/blob/main/screenshot%20from%20my%20local%20en/click%20on%20build%20now.PNG)

### Step 3: Set Up Build Trigger via Webhook
1. Open **Configure** on your Jenkins job.
2. Enable **GitHub hook trigger for GITScm polling** under **Build Triggers**.

![screenshot](https://github.com/Prince-Tee/Tooling-Website-deployment/blob/main/screenshot%20from%20my%20local%20en/click%20on%20configure%20then%20scroll%20down%20and%20click%20on%20github%20hook.PNG)

![screenshot](https://github.com/Prince-Tee/Tooling-Website-deployment/blob/main/screenshot%20from%20my%20local%20en/build%20completed.PNG)

3. Add **Archive the artifacts** under **Post-build Actions**.

![screenshot](https://github.com/Prince-Tee/Tooling-Website-deployment/blob/main/screenshot%20from%20my%20local%20en/click%20on%20add%20post%20build%20then%20click%20on%20achive%20the%20artifacts.PNG)

### Step 4: Test Webhook Trigger  
Make a small change to the `README.md` in the GitHub repo. This should automatically trigger a new Jenkins build.

![screenshot](https://github.com/Prince-Tee/Tooling-Website-deployment/blob/main/screenshot%20from%20my%20local%20en/going%20to%20github%20to%20change%20the%20readme%20so%20we%20can%20test%20with%20it.PNG)

### Step 5: Troubleshoot Errors  
- If the build fails, check the console output for errors (such as misconfigurations).

## 3. Copy Artifacts to NFS Server via SSH

### Step 1: Install Publish Over SSH Plugin  
1. Navigate to **Manage Jenkins** > **Plugins** > **Available Plugins**.
2. Search and install **Publish Over SSH**.
3. Restart Jenkins.

![screenshot](https://github.com/Prince-Tee/Tooling-Website-deployment/blob/main/screenshot%20from%20my%20local%20en/once%20successful%20restart%20jenkins.PNG)

4. then login afresh

![screenshot](https://github.com/Prince-Tee/Tooling-Website-deployment/blob/main/screenshot%20from%20my%20local%20en/login%20afresh%20on%20jenkins.PNG)

### Step 2: Configure SSH to NFS  
1. Open **Manage Jenkins** > **System**, and scroll to **Publish over SSH**.
2. Add your NFS server:
   - Provide the private key, server name (`NFS-server`), and remote directory (`/mnt/apps`).
   - Ensure that Jenkins has SSH access to the NFS server.

### Step 3: Post-Build Action  
1. Go to **Post-build Actions** and configure **Send build artifacts over SSH**.
2. Define the directory `/mnt/apps` and use `**` to represent all files.

![screenshot](https://github.com/Prince-Tee/Tooling-Website-deployment/blob/main/screenshot%20from%20my%20local%20en/send%20build%20to%20ssh.PNG)

### Step 4: Test the Configuration  
1. Modify the `README.md` again in the GitHub repo.
2. Confirm successful build and transfer of artifacts to `/mnt/apps` on the NFS server.

## Troubleshooting

- **SSH Issues**: Ensure the correct security group and SSH settings between Jenkins and NFS.
- **File Permissions**: Verify permissions and ownership for the `/mnt/apps` directory on NFS.

## Conclusion  
In this guide, we've automated Jenkins builds triggered by GitHub webhooks and transferred build artifacts to an NFS server via SSH. This is a foundation for more complex CI/CD tasks.

---

This documentation follows the steps and structure from your original write-up but adapts it for clear readability on GitHub. Let me know if you need further adjustments or screenshots!