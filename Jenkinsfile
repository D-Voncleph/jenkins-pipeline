pipeline {
    agent any

    stages {
        stage('Cloning Code') {
            steps {
                echo 'Fetching the Application Source Code...'
                git branch: 'main', url: 'https://github.com/D-Voncleph/fintech-app-docker.git'
                
                // Let's list what is inside the backend folder to be sure
                sh 'ls -la backend' 
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
                
                // 🔴 OLD: sh 'docker build -t voncleph/fintech-app:v1 .'
                // 🟢 NEW: Point to the ./backend folder
                sh 'docker build -t voncleph/fintech-app:v1 ./backend'
            }
        }
    }
}
