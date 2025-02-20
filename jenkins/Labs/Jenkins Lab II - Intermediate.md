
<div style="text-align:center; font-size: 24px;">
<p><strong>🔥 ADVANCED JENKINS TRAINING 🔥</strong></p>
</div>
<div style="text-align:center; font-size: 20px;">
<p><strong>📌Laboratory II Intermediate - Plugins, Pipelines & Deployment</strong></p>
</div>

## 🎯 By the end of this lab, trainess will:

- Install and configure essential Jenkins plugins.
- Differentiate between Declarative and Scripted Pipelines.
- Connect Jenkins to a Git repository for automated builds.
- Integrate build and test stages within a Jenkins pipeline.
- Deploy artifacts using Docker from a Jenkins pipeline.
- Secure Jenkins with credential management for external integrations.

---

## 1️⃣ Jenkins Plugins
### ✅ Objective: Install and configure essential Jenkins plugins.  
---
**Prerequisites**: Jenkins instance with admin access.

🔹 **Tasks:**

-  **Install Plugins**:
   - Navigate to **Dashboard > Manage Jenkins > Plugins > Available plugins**.
   - Install the following plugins (if not exist):  
     - Docker
     - Docker API
     - Docker Common  
     - Docker Pipeline 

- **Configure Credentials**:
   - Go to **Dashboard > Manage Jenkins > Credentials**
   - Add a new `Username with Password` credential for your Git repository.
   - Provide a name for the Git ID (e.g., git-credentials).
   - Add a new `Username with Password` credential for your Docker hub.
   - Provide a name for the Docker Hub ID (e.g., docker-hub-credentials).
   
---

## 2️⃣Declarative vs Scripted Pipeline
### ✅ Objective: Write and compare Declarative and Scripted Pipelines.

🔹 **Tasks:**

- **Declarative Pipeline**:
   - Create a new Pipeline job with name **DeclarativePipeline**
   - Add **Build stage** with message "Building...."
     ```groovy
    
     
     ```
   - Run the pipeline and observe the output.

- **Scripted Pipeline**:
   - Create another Pipeline job with name **ScriptedPipeline** 
   - Add **Build stage** with message "Building...."
     ```groovy
   
     ```
   - Run the pipeline and compare the syntax with the Declarative example.

---

## 3️⃣ Connect Jenkins to a Git Repository
### ✅ Objective: Configure Jenkins to pull code from a Git repository.  
**Prerequisites**: A GitHub account and a sample repository (e.g., [https://github.com/devopsdelivery/yoda](https://github.com/devopsdelivery/yoda)).

🔹 **Tasks:**

- Create a new Pipeline job.
- Under **Pipeline > Definition**, select "Pipeline script from SCM".
- Configure:
   - **SCM**: Git  
   - **Repository URL**: Your Git repo URL (e.g., `https://github.com/devopsdelivery/yoda.git`)  
   - **Credentials**: Select the credentials created in Exercise 1.  
- Save and run the job. Verify that Jenkins clones the repository.

---

## 4️⃣  First Build and Code Testing
### ✅ Objective: Run a build and integrate automated tests.

🔹 **Tasks:**

- Modify the Declarative Pipeline from Exercise 2 to include a test stage:
   ```groovy
   pipeline {
     agent any
     stages {
       stage('Build') {
         steps { 
            echo 'Building...' 
             // Replace with actual build command (`mvn clean package`)
            } 
            }
       stage('Test') {
         steps {
           sh 'echo "Running unit tests..."'
           // Replace with actual test command (`mvn test`)
         }
       }
     }
   }
  ```
---

## 5️⃣ Artifact Deployment with Docker

### ✅ Objective: Deploy artifacts using Docker in Jenkins.  

🔹 **Tasks:**

#### - Configure Docker in Jenkins:
- Access the Jenkins container:
```bash
   docker ps
   docker exec -it <CONTAINER_ID> /bin/bash
   ```

- Update packages and install Docker CLI:
```bash
   apt-get update && apt-get install -y docker.io
   ```
   Nota: For permission issue, execute this command:
    
  ```bash
   docker exec -u root -it <CONTAINER_ID> bash
  ```
- Create a Docker group and add the Jenkins user:
```bash
groupadd docker
usermod -aG docker jenkins
chown root:docker /var/run/docker.sock
chmod 666 /var/run/docker.sock
```
- Exit in Jenkins container:
```bash
 exit
 ```
- Restart the Jenkins container:
```bash
docker restart <CONTAINER_ID>
```

- Add create Docker file in your root project
- Create a Deployment Pipeline:
```groovy
pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = '{{YOUR_GIT_CREDENTIAL_ID}}' //
        IMAGE_NAME = '{{TOUR_DOCKER_USER_NAME}}/{{given_image_name}}'
    }

     /** TODO 
    1. add clean up stage
    2. add clone repository stage
    3. add maven test stage
    4. add maven build stage
   
    */

        stage('Docker Build') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}")
                }
            }
        }

        stage('Docker Push') {
            steps {
                script {
                    docker.withRegistry('', "${DOCKERHUB_CREDENTIALS}") {
                        docker.image("${IMAGE_NAME}").push("latest")
                    }
                }
            }
        }
    
    post {
        always {
            junit '**/target/surefire-reports/*.xml' // Tests Report
            archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true // arqquive the JAR file
        }
        success {
            echo 'Deployment completed successfully on Docker Hub!'
        }
        failure {
            echo 'The deployment failed. Check the logs.'
        }
    }
}
```
- Pull Docker image created 
```bash
docker pull index.docker.io/{{TOUR_DOCKER_USER_NAME}}/{{given_image_name}}:latest
```
- Create docker container with non-detached mode:
```bash
docker run --name java-app index.docker.io/{{TOUR_DOCKER_USER_NAME}}/{{given_image_name}}:latest
 ```



