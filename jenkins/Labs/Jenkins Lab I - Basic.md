<div style="text-align:center; font-size: 24px;">
<p><strong>🔥 ADVANCED JENKINS TRAINING 🔥</strong></p>
</div>
<div style="text-align:center; font-size: 20px;">
<p><strong>📌Laboratory I Basic -  Jenkins Set up & Basics</strong></p>
</div>

<br>

## 🎯 By the end of this lab, trainess will:

Install and configure Jenkins using Docker.

Create and execute a Freestyle Job.

Build and execute a basic Jenkins pipeline.

Integrate Jenkins with GitHub for automated builds.

Work with pipeline parameters for dynamic configurations.

Set up a Multibranch Pipeline to manage different Git branches.

Clean up the Jenkins environment for future setups.


## 1️⃣ Installing and Configuring Jenkins

### ✅ Objective: Install and configure Jenkins using Docker.

 ---

🔹 **Tasks:**

- Install Docker and Visual Code on your machine (if not already installed). 🐳 <br>
  [Docker Desktop for Windows Donwload](https://docs.docker.com/desktop/setup/install/windows-install/) </br>
  [VS Code Download](https://code.visualstudio.com/Download)
  <br>

- Run the following command to start a Jenkins container:
  <br>
  
  ```sh
  docker run -d -p 8080:8080 -p 50000:50000 --restart=on-failure -v /var/run/docker.sock:/var/run/docker.sock -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts-jdk17
  ```
- Access [http://localhost:8080](http://localhost:8080) and enter the password displayed in the terminal. 🔑 <br>
 
   Note: If the password is not displayed in the terminal, enter the following commands in the command line:<br>
    
```sh 
    docker ps
    docker exec -it <CONTAINER_ID> cat /var/jenkins_home/secrets/initialAdminPassword
  ```
- Complete the initial Jenkins setup by Installing suggested plugins. ⚙️

- Open the terminal on your local machine and install Maven where Jenkins is running.
 
 ```sh
  docker exec -it -u root <CONTAINER_ID> /bin/bash
  apt-get update
  apt-get install maven
 ```
📌 **Question:** What happens if you restart the container? Does Jenkins retain the configurations? 🤔 <br>
 

---

## 2️⃣ Creating and Running a Jenkins Job

### ✅ Objective: Create a Freestyle Job and execute a simple command.
🔹 **Tasks:**

- Create a new job in Jenkins and name it **MyFirstFreestyleJob**. 📝
- Select **"Freestyle Project"**.
- In the **"Build Steps"** section, add **"Execute Shell"** and enter the command:
  ```sh
  echo "Hello, Jenkins!"
  ```
- Save and run the job. ▶️
- Open the build
- Click in **Console Output**. 🖥️

📌 **Question:** Where is the job execution history stored? 📂 <br>

---

## 3️⃣ Creating a Jenkins Pipeline

### ✅ Objective: Create a basic pipeline and understand its components.
🔹 **Tasks:**

- Create a new pipeline named **TestPipeline**. ⛓️
-  Select ➕ New Item -> Pipeline
- In the **"Pipeline"** section, add **"Pipeline script"** and enter the command:
  ```groovy
  pipeline {
      agent any
      stages {
        stage('clean workdir') {
              steps {
                  echo 'Clean workdir...'
              }
          }
          stage('Build') {
              steps {
                  echo 'Building the project...'
              }
          }
          stage('Test') {
              steps {
                  echo 'Running tests...'
              }
          }
          stage('Deploy') {
              steps {
                  echo 'Deploying application...'
              }
          }
      }
  }
  ```
- Save and run the pipeline. 🚀

📌 **Question:** What is the difference between a Freestyle Job and a Pipeline based in you point of view? 🔍

---

## 4️⃣ Integrating with GitHub

### ✅ Objective: Configure Jenkins to automatically clone a GitHub repository.
🔹 **Tasks:**

- Create a new pipeline named **CloneRepo**. 🔄
-  Select ➕ New Item -> Pipeline
- In the **Pipeline** section, add **Pipeline script** and enter the command
- Add the following code:
  ```groovy
  pipeline {
      agent any
      stages {
          stage('Clone repository') {
              steps {
                  git 'https://github.com/devopsdelivery/simple-java-maven-app.git'
              }
          }
      }
  }
  ```
- Save and run the pipeline. ✅
- Open the build
- Click on WorkSpace and verify if the files were cloned correctly 📁

📌 **Question:** What happens if the repository is private? 🔒

---

## 5️⃣ Working with Pipeline Parameters

### ✅ Objective: Create a pipeline that accepts user input parameters.
🔹 **Tasks:**
- Create a pipeline with a user input parameter:
- Select ➕ New Item -> **PipelineWithParameter**
- In the **Pipeline** section, add **Pipeline script** and add the following code:
  ```groovy
  pipeline {
      agent any
      parameters {
          string(name: 'USERNAME', defaultValue: 'user', description: 'Enter your name')
      }
      stages {
          stage('Print Name') {
              steps {
                  echo "Welcome, ${USERNAME}!"
              }
          }
      }
  }
  ```
- Access Jenkins and try providing your name and running the project.
- Create a pipeline with user-chosen parameters and display message based on the selected parameters (replace the previous pipeline).
```groovy
  pipeline {
  agent any
    parameters {
        choice(choices: ['TEST', 'DEV', 'QA'], description: 'Which environment to deploy in?', name: 'deployEnv')
    }
    stages {
        stage('Deploy to Environment') {
            steps {
                script {
                    
                     {
                        
                        /* Display message based on select parameters:
                            TEST -> Deploying to TEST environment...
                            DEV -> Deploying to DEV environment...
                            QA -> Deploying to QA environment...
                        */

                    }
                }
            }
        }
  }
} 
```
- Run the pipeline and test different values. 🎭

📌 **Question:** How do parameters improve pipeline reusability? 🔄

---

## 6️⃣ Multibranch Pipelines

### ✅ Objective: Configure a Multibranch Pipeline to handle different branches of a Git repository.
🔹 **Tasks:**

- Create a new **Multibranch Pipeline** named **MultiBranch-Test**. 🌿
- In the **Branch Sources** section, select **Git** and add your Git repository
- Configure it to monitor changes for each 5min (H/5 * * * *) 🔄
- the following code:Configure it to monitor in Branch Sources by selecting a Git 
- In your `Jenkinsfile` in the repository, add the following content:
  ```groovy
  pipeline {
      agent any
      stages {
          stage('Branch Info') {
              steps {
                  echo "Current branch: ${env.BRANCH_NAME}"
              }
          }
      }
  }
  ```
- Test if the pipeline executes correctly for each branch. ✅

📌 **Question:** Based on previous exercise, how does Jenkins automatically discover new branches? 🤖

---


## 7️⃣ Cleanup (Environment Cleanup)

### ✅ Objective: Remove Jenkins and its data to start from scratch.
🔹 **Tasks:**

- Stop and remove the Jenkins container:
  ```sh
  docker container stop <CONTAINER_ID>
  docker container rm <CONTAINER_ID>
  ```
- List and remove Docker volumes:
  ```sh
  docker volume ls
  docker volume rm jenkins_home
  ```
- Verify if Jenkins was completely removed. 🗑️

📌 **Question:** What is the impact of deleting Docker volumes on Jenkins? ⚠️ <br>


---
<br>
<br>
                                                                                           
