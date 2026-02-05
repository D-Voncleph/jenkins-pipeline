pipeline {
    agent any

    stages {
        stage('Hello Stage') {
            steps {
                echo 'Hello World! This is my first Pipeline.'
                sh 'echo "Current user is:"'
                sh 'whoami'
            }
        }
    }
}
