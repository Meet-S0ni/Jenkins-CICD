# Automating the Deployment Lifecycle with Jenkins and Kubernetes

This project showcases the automation of the deployment lifecycle using Jenkins and Kubernetes. 
By following a set of predefined steps, developers can seamlessly deploy their code changes. 
The pipeline includes actions like GitHub integration, Docker image management, manifest.yaml versioning, 
and Kubernetes environment provisioning, resulting in a robust and reliable development and deployment process

## Workflow Steps

**1. Developer Pushes Code to GitHub**: Developers push their code to a GitHub repository.

**2. Jenkins Clones Repository**: Jenkins clones the GitHub repository to its environment.

**3. Jenkins Builds Docker Image**: Jenkins builds a Docker image from the source code.

**4. Jenkins Pushes Image to Docker Hub**: The Docker image is pushed to Docker Hub.

**5. Update Image Tag in manifest.yaml**: Jenkins updates the image tag in the `manifest.yaml` file with the latest build tag.

**6. Jenkins Pushes Code to GitHub**: Jenkins pushes all code changes, including the updated `manifest.yaml`, back to the GitHub repository.

**7. Jenkins SSH Into Kubernetes Server**: Jenkins establishes an SSH connection to a Kubernetes server.

**8. Jenkins Copies manifest.yaml to Kubernetes Server**: The `manifest.yaml` file is copied to the Kubernetes server using SCP.

**9. Jenkins Applies Manifest to Kubernetes Environment**: Jenkins applies the updated manifest file to the Kubernetes environment, deploying the web application.

## Jenkinsfile

https://github.com/Meet-S0ni/Jenkins-CICD/blob/main/Jenkinsfile

## Jenkins Configuration Steps

Follow these steps to set up the Jenkins CI/CD pipeline:

**Step:1. Install Jenkins**: 
   - for jenkins you requires jdk:8 or jdk:11 
   - install jenkins package as per you operating system
   - you can refer: https://www.jenkins.io/doc/book/installing/

**Step:2. Set Up GitHub Repository**:
   - Create a GitHub repository to store your code.
   - Obtain a GitHub username and password of PAT for Jenkins to access the repository.

**Step:3. Install Docker and Docker Pipeline Plugin**: 
   - Ensure Docker is installed on your Jenkins server, and install the Docker Pipeline plugin.

**Step:4. Configure Docker Registry Credentials**:
   - In Jenkins, go to 
   ``` bash 
   "Manage Jenkins" -> "Manage Credentials" -> "Global credentials (unrestricted)".
   ```
   - Add Docker Hub registry credentials with the ID `dockerhub`.

**Step:5. Create a New Jenkins Job**:
   - Create a new Jenkins job and configure it as a Pipeline job.
   - In the job configuration, specify the `Jenkinsfile` from your code repository.

**Step:6. Configure Jenkins Build Parameters**:
   - Set up Jenkins build parameters as needed, such as `DOCKERTAG`, `GIT_USERNAME`, and `GIT_PASSWORD`.

**Step:7. Configure SSH Credentials**:
   - In Jenkins, go to "Manage Jenkins" -> "Manage Credentials".
   - Add SSH credentials for connecting to your Kubernetes server, and set the ID in the Jenkinsfile.

**Step:8. Install Necessary Plugins**:
   - Depending on your specific requirements, install any additional Jenkins plugins required for your CI/CD workflow.

**Step:9. Run the Jenkins Job**:
   - Trigger the Jenkins job manually or set up webhooks or triggers as needed to automate the workflow when code is pushed to the GitHub repository.

**Step:10. Monitor and Debug**:
    - Monitor your Jenkins job for successful or failed builds.
    - Debug any issues by checking the console output and logs.

By following these steps, you can set up and configure Jenkins to automate your CI/CD workflow for your web application.

