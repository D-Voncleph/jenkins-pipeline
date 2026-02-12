pipeline {
    agent any

    stages {
        stage('Cloning Code') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }

        stage('Running Tests') {
            steps {
                echo 'Simulating tests...'
                sh 'echo "Tests Passed!"'
            }
        }

        // NEW STAGE ADDED HERE
        stage('Build Docker Image') {
            steps {
                echo 'Building the Docker image...'
                // Replace 'voncleph' with your actual Docker Hub username!
                sh 'docker build -t voncleph/fintech-app:v1 .'
            }
        }
    }
}
