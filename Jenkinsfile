pipeline {
    agent any

    stages {
        stage('Cloning Code') {
            steps {
                echo 'Fetching the Application Source Code...'
                // 🔴 OLD: checkout scm
                // 🟢 NEW: Pull the ACTUAL App Code from your fintech repo
                git branch: 'main', url: 'https://github.com/D-Voncleph/fintech-app-docker.git'
                
                // Verify that Dockerfile is now present
                sh 'ls -la'
            }
        }

        stage('Running Tests') {
            steps {
                echo 'Simulating tests...'
                sh 'echo "Tests Passed!"'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building the Docker image...'
                // This will now work because we downloaded the code above!
                sh 'docker build -t voncleph/fintech-app:v1 .'
            }
        }
    }
}
