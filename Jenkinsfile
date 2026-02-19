pipeline {
    agent any

    // 1. Define Global Variables
    environment {
        // Change this to your Docker Hub username/repo name
        REGISTRY_IMAGE = "voncleph/fintech-app" 
        DOCKER_CREDENTIALS_ID = "docker-hub-credentials"
    }

    stages {
        stage('Cloning Code') {
            steps {
                echo 'Fetching the Application Source Code...'
                // Pulls the code from your backend repo
                git branch: 'main', url: 'https://github.com/D-Voncleph/fintech-app-docker.git'
            }
        }

        stage('Running Tests') {
            steps {
                echo 'Simulating tests...'
                sh 'echo "Simulating a critical test failure!"; exit 1'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building the Docker image...'
                // Builds the image and tags it as "latest" locally
                // Note: We point to ./backend because that's where your Dockerfile is
                sh "docker build -t ${REGISTRY_IMAGE}:latest ./backend"
            }
        }

        // 🔴 NEW STAGE: PUSH TO HUB
        stage('Push to Docker Hub') {
            steps {
                echo 'Pushing image to Docker Hub...'
                
                script {
                    // 1. Unlock the secret credentials from Jenkins
                    withCredentials([usernamePassword(credentialsId: DOCKER_CREDENTIALS_ID, usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                        
                        // 2. Log in (Securely - password is masked)
                        sh 'echo $PASS | docker login -u $USER --password-stdin'
                        
                        // 3. Tag the image with the Jenkins BUILD_NUMBER (e.g., v34)
                        // This creates a unique version for every run.
                        sh "docker tag ${REGISTRY_IMAGE}:latest ${REGISTRY_IMAGE}:v${BUILD_NUMBER}"
                        
                        // 4. Push both the 'latest' tag and the specific version tag
                        sh "docker push ${REGISTRY_IMAGE}:latest"
                        sh "docker push ${REGISTRY_IMAGE}:v${BUILD_NUMBER}"
                    }
                }
            }
        }
    }
    
    // Cleanup: Remove the image from the Jenkins server to save space
    post {
        always {
            sh "docker logout"
        }
    }
}
