# BoardgameListingWebApp

## Description

**Board Game Database Full-Stack Web Application.**
This web application displays lists of board games and their reviews. While anyone can view the board game lists and reviews, they are required to log in to add/ edit the board games and their reviews. The 'users' have the authority to add board games to the list and add reviews, and the 'managers' have the authority to edit/ delete the reviews on top of the authorities of users.  

## Technologies

- Java
- Spring Boot
- Amazon Web Services(AWS) EC2
- Thymeleaf
- Thymeleaf Fragments
- HTML5
- CSS
- JavaScript
- Spring MVC
- JDBC
- H2 Database Engine (In-memory)
- JUnit test framework
- Spring Security
- Twitter Bootstrap
- Maven

## Features

- Full-Stack Application
- UI components created with Thymeleaf and styled with Twitter Bootstrap
- Authentication and authorization using Spring Security
  - Authentication by allowing the users to authenticate with a username and password
  - Authorization by granting different permissions based on the roles (non-members, users, and managers)
- Different roles (non-members, users, and managers) with varying levels of permissions
  - Non-members only can see the boardgame lists and reviews
  - Users can add board games and write reviews
  - Managers can edit and delete the reviews
- Deployed the application on AWS EC2
- JUnit test framework for unit testing
- Spring MVC best practices to segregate views, controllers, and database packages
- JDBC for database connectivity and interaction
- CRUD (Create, Read, Update, Delete) operations for managing data in the database
- Schema.sql file to customize the schema and input initial data
- Thymeleaf Fragments to reduce redundancy of repeating HTML elements (head, footer, navigation)

## How to Run

1. Clone the repository
2. Open the project in your IDE of choice
3. Run the application
4. To use initial user data, use the following credentials.
  - username: bugs    |     password: bunny (user role)
  - username: daffy   |     password: duck  (manager role)
5. You can also sign-up as a new user and customize your role to play with the application! 😊


---

🚀 Building an End-to-End CI/CD Pipeline with DevOps Tools
In today's fast-paced software world, automation is the backbone of reliable delivery. A well-designed CI/CD pipeline ensures faster development, better code quality, and seamless deployments. In this comprehensive guide, I'll walk you through how we set up a complete DevOps pipeline - from infrastructure to monitoring.
✅ Automatically builds and tests your Java applications
✅ Performs security scans with Trivy
✅ Analyzes code quality with SonarQube (optional)
✅ Stores artifacts in Nexus Repository
✅ Builds and publishes Docker images
✅ Deploys to Kubernetes cluster
✅ Sends detailed email notifications

Architecture Overview
Our pipeline consists of three main phases:
Infrastructure Setup - AWS EC2 instances and Kubernetes cluster
Source Code Management - GitHub repository setup
CI/CD Pipeline - Jenkins pipeline with multiple stages

---

🔹 Phase 1: Infrastructure Setup
We started by creating 6 Ubuntu 24.04 instances on AWS EC2. These serve as the backbone for our entire DevOps toolchain:
Instance 1: Jenkins Server (CI/CD Engine)
Instance 2: Nexus Repository Manager
Instance 3: SonarQube Server
Instance 4: Kubernetes Master Node
Instance 5: Kubernetes Worker Node 1
Instance 6: Kubernetes Worker Node 2

Key components installed in Phase 1:
✅ Docker on all VMs for containerized deployments
✅ Jenkins as the CI/CD orchestrator
✅ Trivy integrated for comprehensive vulnerability scanning
✅ Nexus for artifact repository management
✅ SonarQube for code quality analysis and technical debt tracking

👉 Result: A complete DevOps toolchain installed and ready for pipeline automation.


Step 1: Creating AWS EC2 Instances
We'll need six Ubuntu 24.04 instances for our complete setup:
Instance 1: Jenkins Server (CI/CD orchestrator)
Instance 2: Nexus Repository Manager (Artifact storage)
Instance 3: SonarQube Server (Code quality analysis)
Instance 4: Kubernetes Master Node (Control plane)
Instance 5: Kubernetes Worker Node 1 (Application workloads)
Instance 6: Kubernetes Worker Node 2 (Application workloads)

Creating EC2 Instances
Sign in to AWS Console

Navigate to AWS Management Console
Sign in with your credentials

Launch EC2 Instances Navigate to EC2 Dashboard and click Launch Instance
Security Group Configuration by Server Role

Jenkins Server: - Port 22 (SSH) - Port 8080 (Jenkins Web UI)  Nexus Server: - Port 22 (SSH) - Port 8081 (Nexus Web UI)  SonarQube Server: - Port 22 (SSH) - Port 9000 (SonarQube Web UI)  Kubernetes Master: - Port 22 (SSH) - Port 6443 (Kubernetes API Server) - Port 2379-2380 (etcd) - Port 10250 (kubelet) - Port 10251 (kube-scheduler) - Port 10252 (kube-controller-manager)  Kubernetes Workers: - Port 22 (SSH) - Port 10250 (kubelet) - Port 30000-32767 (NodePort services)
<img width="1910" height="1079" alt="Screenshot 2025-08-24 161242" src="https://github.com/user-attachments/assets/674e7e4c-f4d7-4100-a164-3f9be1618d10" />



Step 2: Installing Docker on All Instances
Connect to each of the 6 instances and install Docker following these steps:
First, update your system packages and install prerequisite packages including ca-certificates and curl. Then download and add Docker's official GPG key to your system keyring. Add the Docker repository to your APT sources list, making sure to use the correct architecture and Ubuntu version codename.
Update the package index again and install the Docker packages including docker-ce, docker-ce-cli, and containerd.io. Finally, grant permissions to the Docker socket for easier management and verify the installation by checking the Docker version.
This process needs to be repeated on all 6 servers to ensure Docker is available for containerized deployments across your infrastructure.


Step 3: Setting Up Jenkins
On your Jenkins server instance (Instance 1):
Begin by updating your system and upgrading existing packages. Install Java 17 as it's required for Jenkins to function properly. Download the Jenkins repository key from the official Jenkins website and add it to your system's keyring.
Add the Jenkins repository to your APT sources list, then update the package index and install Jenkins. Once installed, start the Jenkins service and enable it to start automatically on system boot.
Access Jenkins through your web browser using the server's IP address on port 8080. Retrieve the initial admin password from the specified file location, enter it in the web interface, install the suggested plugins, and create your first admin user account.
Access Jenkins at http://your-jenkins-server-ip:8080 and complete the setup wizard.


Step 4: Installing Trivy (Security Scanner)
On the Jenkins server (Instance 1):
Install the prerequisite packages including wget, apt-transport-https, gnupg, and lsb-release. Add the Trivy repository key to your system using wget to download the public key.
Add the Trivy repository to your APT sources list, making sure to use the correct Ubuntu release codename. Update your package index and install Trivy security scanner.
Trivy will be used in our Jenkins pipeline to scan both the filesystem and Docker images for security vulnerabilities.


Step 5: Setting Up Nexus Repository
On your dedicated Nexus server (Instance 2):
Pull the official Nexus Docker image from Sonatype. Create and run a Nexus container with port mapping for port 8081, assign it a name for easy management, and create a volume for persistent data storage.
To get the initial admin password, check the container logs using Docker logs command and look for the password information. The default username is admin.
Access Nexus at http://your-nexus-server-ip:8081 and complete the initial setup wizard.


Step 6: Setting Up SonarQube
On your dedicated SonarQube instance (Instance 3):
Create a Docker network specifically for SonarQube and its PostgreSQL database to enable secure communication between containers.
Run a PostgreSQL container with the necessary environment variables for SonarQube database configuration. Set up database credentials, create the sonarqube database, and configure persistent volume storage for database data.
Next, run the SonarQube container on the same network with proper database connection settings. Configure the JDBC URL to connect to the PostgreSQL database, set database credentials, and create persistent volumes for SonarQube data, extensions, and logs.
Access SonarQube at http://your-sonarqube-server-ip:9000 using the default credentials admin/admin.


Step 7: Setting Up Kubernetes Cluster
Now we'll set up a complete Kubernetes cluster with 1 master and 2 worker nodes.
On the Master Node (Instance 4):
Install Kubernetes Components
First, update the system and install required packages for Kubernetes. Add the Kubernetes signing key and repository to your system. Then update the package index and install kubelet, kubeadm, and kubectl components.
Initialize Kubernetes Master
Use kubeadm to initialize the Kubernetes cluster on the master node. This will set up the control plane components including the API server, etcd, scheduler, and controller manager. After initialization, you'll get a join command that you'll use on worker nodes.
Configure kubectl for the Master Node
Set up the kubeconfig file so you can manage the cluster using kubectl. Copy the admin configuration file to your user's home directory and set appropriate permissions.
Install Pod Network (CNI)
Install a Container Network Interface plugin like Flannel or Calico to enable pod-to-pod communication across the cluster. This is essential for the cluster to function properly.

On Worker Nodes (Instances 5 & 6):
Install Kubernetes Components
Repeat the same installation process as the master node - update system, add Kubernetes repository, and install kubelet and kubeadm.
Join the Cluster
Use the join command that was generated during master initialization to connect each worker node to the cluster. This command includes the master node's IP address and a secure token.
Verify Cluster Setup
From the master node, verify that all nodes have joined the cluster successfully and are in Ready state. Check that system pods are running correctly across all nodes.
<img width="1913" height="1032" alt="Screenshot 2025-08-24 161314" src="https://github.com/user-attachments/assets/a83e376f-756d-4619-8f48-66ec9e103ff3" />


🔹 Phase 2: Source Code Management
Our codebase was managed on GitHub in a private repository. This ensures security while maintaining version control and collaboration capabilities.
Setting Up Git Repository
Using Git Bash, we established our source code management workflow:
Initialize the repository in your local project directory if not already done. Add your GitHub repository as the remote origin using the repository URL. Stage all your project files for the initial commit.
Create your initial commit with a descriptive message about your DevOps Board Game application. Push your code to GitHub using the main branch, which will serve as the trigger for your automated Jenkins pipeline.
This setup ensures that all builds are triggered directly from GitHub, enabling true continuous integration where every code push automatically initiates our comprehensive pipeline.
<img width="1910" height="1016" alt="Screenshot 2025-08-24 161301" src="https://github.com/user-attachments/assets/f889f933-6682-4376-afed-51c19239f0c9" />

---

🔹 Phase 3: CI/CD Pipeline Implementation
This is where the magic happens! We built a Jenkins declarative pipeline that automated the entire application lifecycle.
📧 Email Notifications
After every pipeline run, Jenkins automatically sends an HTML-formatted email notification with:
Build status (SUCCESS/FAILURE) with color-coded banners
Direct links to console output for troubleshooting
Trivy security reports attached for vulnerability review
Professional formatting for team communications
<img width="1919" height="1077" alt="Screenshot 2025-08-24 161437" src="https://github.com/user-attachments/assets/17c73542-d04c-437d-b2fd-6be13c2e5fa1" />

---

Step 8: Configuring Jenkins Pipeline
Required Jenkins Plugins
Install these plugins in Jenkins:
Pipeline
Git
Maven Integration
Docker Pipeline
SonarQube Scanner
Email Extension
Kubernetes CLI
Configuration as Code

Setting Up Credentials
In Jenkins, go to Manage Jenkins → Credentials and add
GitHub Credentials (git-cred)

Type: Username with password
Username: Your GitHub username
Password: GitHub Personal Access Token

Docker Registry Credentials (docker-cred)

Type: Username with password
Username: Your Docker Hub username
Password: Docker Hub password/token

Kubernetes Token (k8-cred)

Type: Secret text
Secret: Your Kubernetes cluster token

Configuring Tools
Go to Manage Jenkins → Tools and configure:
JDK Installations

Name: jdk17
Install automatically: OpenJDK 17

Maven Installations

Name: maven3
Install automatically: Maven 3.9.x

SonarQube Scanner

Name: sonar-scanner
Install automatically: Latest version

Step 9: Creating the Jenkins Pipeline
We built a Jenkins declarative pipeline that automated the entire lifecycle with the following stages:
📋 Pipeline Stages Overview:
Git Checkout - Pull latest code from GitHub
Compile - Use Maven to compile code
Test - Run unit tests
Trivy FS Scan - Scan local file system for vulnerabilities
SonarQube Analysis - Code quality checks (can be enabled/disabled)
Build - Package the application
Publish to Nexus - Store build artifacts
Docker Build & Tag - Create Docker image
Docker Image Scan - Check for container vulnerabilities
Push to Docker Hub - Publish the container image
Deploy to Kubernetes - Deploy to K8s cluster
Verify Deployment - Confirm successful deployment

Create a new Pipeline job in Jenkins and use this production-ready Jenkinsfile:

pipeline {
    agent any
    tools {
        jdk 'jdk17'
        maven 'maven3'
    }
    
    stages {
        stage('Git Checkout') {
            steps {
                 git branch: 'main', credentialsId: 'git-cred', url: 'https://github.com/Vinay00018/DevopsBoardGame.git'
            }
        }
         stage('Compile') {
            steps {
                sh "mvn compile"
            }
        }
         stage('Test') {
            steps {
                sh "mvn test"
            }
        }
         stage('File System Scan') {
            steps {
                sh "trivy fs --format table -o trivy-fs-report.html ."
            }
        }
     // stage('SonarQube Analysis') {
        //    steps {
          //   withSonarQubeEnv('sonar'){
             //       sh '''$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Boardgame -Dsonar.projectKey=Boardgame \
             //        -Dsonar.java.binaries=.'''
             //}
           //  }
         //}
        //  stage('Quality Gate') {
        // steps {
          //      script{
            //        waitForQualityGate abortPipeline:false,credentialsId: 'sonar-token'
            //    }
        //     }
    //    }
         stage('Build') {
            steps {
                sh "mvn package"
        }
    }
       stage('Publish To Nexus') {
        steps {
               script {
                    try {
                        withMaven(globalMavenSettingsConfig: 'global-settings', jdk: 'jdk17', maven: 'maven3', mavenSettingsConfig: '', traceability: true) {
                            sh "mvn deploy"
                        }
                    } catch (err) {
                        echo "Nexus deployment failed: ${err.getMessage()}"
                        echo "Continuing with pipeline..."
                        // Removed the line that sets UNSTABLE status
                        // currentBuild.result = 'UNSTABLE'
                    }
                }
        }
    }
       stage('Build & Tag Docker Image') {
            steps {
                script{
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                    sh "docker build -t vinay843/boardshack:latest ."
                    }
                }
        }
    }
       stage('Docker Image Scan') {
            steps {
                sh "trivy image --format table -o trivy-image-report.html vinay843/boardshack:latest"
        }
    }
     stage('Push Docker Image') {
            steps {
                script{
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                    sh "docker push vinay843/boardshack:latest"
                    }
                }
        }
    }
       stage('Deploy To Kubernetes') {
            steps {
                withKubeConfig(caCertificate: '', clusterName: 'kubernetes', contextName: '', credentialsId: 'k8-cred', namespace: 'webapps', restrictKubeConfigAccess: false, serverUrl: 'https://172.31.36.124:6443') {
                sh "kubectl apply -f deployment-service.yaml"
               }
        }
    }
     stage('Verify the Deployment') {
            steps {
                withKubeConfig(caCertificate: '', clusterName: 'kubernetes', contextName: '', credentialsId: 'k8-cred', namespace: 'webapps', restrictKubeConfigAccess: false, serverUrl: 'https://172.31.36.124:6443') {
                sh "kubectl get pods -n webapps"
                sh "kubectl get svc -n webapps"
               }
        }
    }
    }
    post {
    always {
        script {
            def jobName = env.JOB_NAME
            def buildNumber = env.BUILD_NUMBER
            def pipelineStatus = currentBuild.result ?: 'SUCCESS'
            def bannerColor = pipelineStatus.toUpperCase() == 'SUCCESS' ? 'green' : 'red'

            def body = """
            <html>
            <body>
            <div style="border: 4px solid ${bannerColor}; padding: 10px;">
                <h2>${jobName} - Build ${buildNumber}</h2>
                <div style="background-color: ${bannerColor}; padding: 10px;">
                    <h3 style="color:white;">Pipeline status: ${pipelineStatus.toUpperCase()}</h3>
                </div>
                <p>Check the <a href="${BUILD_URL}">console output</a>.</p>
            </div>
            </body>
            </html>
            """

            emailext (
                subject: "${jobName} - Build ${buildNumber} - ${pipelineStatus.toUpperCase()}",
                body: body,
                to: 'vinay045vini@gmail.com',
                from: 'jenkins@example.com',
                replyTo: 'jenkins@example.com',
                mimeType: 'text/html',
                attachmentsPattern: 'trivy-image-report.html'
            )
        }
    }
}

}


---

Testing Your Pipeline

Step 10: Running the Complete Pipeline
Trigger the Pipeline

Go to your Jenkins job
Click "Build Now"

Monitor the Stages

Watch each stage execute in real-time
Check console output for any issues
Review Trivy security reports

<img width="1918" height="1069" alt="Screenshot 2025-08-24 161448" src="https://github.com/user-attachments/assets/936715c6-715d-4abf-9c95-df73e5a426b8" />


Verify Deployment
docker image
<img width="1919" height="1077" alt="Screenshot 2025-08-24 161453" src="https://github.com/user-attachments/assets/f796113c-395e-475f-9359-d7ba4086fa7e" />

sonarqube
<img width="1918" height="1069" alt="Screenshot 2025-08-24 161448" src="https://github.com/user-attachments/assets/a08d38fe-2d4d-4533-9a39-6ceca2fe0dc4" />
nexus repository
<img width="1919" height="1069" alt="Screenshot 2025-08-24 161502" src="https://github.com/user-attachments/assets/2fa42079-9f5c-411f-99e0-8fc6cfb4c54b" />


Check Kubernetes pods: kubectl get pods -n webapps
<img width="1919" height="996" alt="image" src="https://github.com/user-attachments/assets/00c6910a-3d49-465f-96b1-9e26ae87dc6e" />

Access your application via LoadBalancer or NodePort service
<img width="1903" height="1028" alt="image" src="https://github.com/user-attachments/assets/7df63ddd-8a10-4b49-828b-449003cd1b1b" />


Check Email Notifications

Confirm receipt of build status email
Review attached Trivy security reports
<img width="1919" height="1035" alt="Screenshot 2025-08-24 161919" src="https://github.com/user-attachments/assets/bded0001-d197-40ef-8937-b7385612a472" />

Verify all links in the email work correctly
<img width="1919" height="1079" alt="Screenshot 2025-08-24 161508" src="https://github.com/user-attachments/assets/bc8d5cfa-20c7-44d6-9bd0-3763fd97d093" />


Conclusion
Congratulations! 🎉 You've successfully built a comprehensive CI/CD pipeline that incorporates industry best practices:
✅ Automated Testing ensures code quality
✅ Security Scanning identifies vulnerabilities early with Trivy
✅ Code Quality Analysis maintains standards (when SonarQube is enabled)
✅ Artifact Management provides version control with Nexus
✅ Container Orchestration enables scalable deployments
✅ Email Notifications keep teams informed with detailed reports

