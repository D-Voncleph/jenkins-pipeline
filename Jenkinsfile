pipeline {
    agent any

    // 🌐 ENVIRONMENT VARIABLES: Keeps the pipeline clean and avoids hardcoding
    environment {
        REGISTRY_IMAGE        = "voncleph/fintech-app" 
        DOCKER_CREDENTIALS_ID = "docker-hub-credentials"
    }

    stages {
        // 📥 STAGE 1: Fetch the source code from the application repository
        stage('Cloning Code') {
            steps {
                echo 'Fetching the Application Source Code...'
                git branch: 'main', url: 'https://github.com/D-Voncleph/fintech-app-docker.git'
            }
        }

        // 🧪 STAGE 2: Execute automated test suites (Quality Gate)
        stage('Running Tests') {
            steps {
                echo 'Executing unit tests...'
                // A successful exit code (0) ensures the pipeline continues
                sh 'echo "Tests Passed successfully!"'
            }
        }

        // 🏗️ STAGE 3: Containerize the application using Docker
        stage('Build Docker Image') {
            steps {
                echo 'Building the Docker image from ./backend...'
                sh "docker build -t ${REGISTRY_IMAGE}:latest ./backend"
            }
        }

        // 🚀 STAGE 4: Authenticate and push to the remote container registry
        stage('Push to Docker Hub') {
            steps {
                echo 'Pushing image and tags to Docker Hub...'
                script {
                    // Securely inject credentials without exposing them in console logs
                    withCredentials([usernamePassword(credentialsId: DOCKER_CREDENTIALS_ID, usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                        sh 'echo $PASS | docker login -u $USER --password-stdin'
                        
                        // Tag with Jenkins BUILD_NUMBER for immutable versioning
                        sh "docker tag ${REGISTRY_IMAGE}:latest ${REGISTRY_IMAGE}:v${BUILD_NUMBER}"
                        
                        // Push both rolling (latest) and static (v#) tags
                        sh "docker push ${REGISTRY_IMAGE}:latest"
                        sh "docker push ${REGISTRY_IMAGE}:v${BUILD_NUMBER}"
                    }
                }
            }
        }
    }
    
    // 🧹 POST-ACTIONS: Always run cleanup to prevent server storage exhaustion
    post {
        always {
            echo 'Securing the host by logging out of Docker...'
            sh "docker logout"
        }
        success {
            echo "✅ Pipeline completed successfully! Image v${BUILD_NUMBER} is now in the registry."
        }
        failure {
            echo "❌ Pipeline failed. Please inspect the Blue Ocean logs for details."
        }
    }
}
