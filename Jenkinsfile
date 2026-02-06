pipeline {
    agent any

    stages {
        // Stage 1: Verify the code is here
        stage('Cloning Code') {
            steps {
                echo 'Step 1: Cloning and verifying repository...'
                // Jenkins automatically clones the code before the pipeline starts.
                // We will list the files to prove it is there.
                sh 'ls -la'
            }
        }

        // Stage 2: Run the dummy tests
        stage('Running Tests') {
            steps {
                echo 'Step 2: Initiating Test Suite...'
                // This is where you would normally run 'npm test' or 'pytest'
                sh 'echo "Tests passed!"'
            }
        }
    }
}
