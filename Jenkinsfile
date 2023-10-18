pipeline {
    agent any // Define the agent for the pipeline (e.g., any available agent)

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Checkout your source code from the repository.
                    checkout([
                        $class: 'GitSCM',
                        branches: [[name: '*/master']], // You can specify the branch you want to build here.
                        userRemoteConfigs: [[url: 'https://github.com/Mohamed-Rouahi/DevopsProject.git']]
                    ])
                }
            }
        }

        stage('Unit Tests') {
            steps {
                // Run 'mvn clean' to clean the project
                sh 'mvn clean'
                // Add more Maven or build commands as needed
            }
        }

        // Add more stages for additional build, test, or deployment steps
    }

    post {
        success {
            // Actions to take if the pipeline succeeds
        }
        failure {
            // Actions to take if the pipeline fails
        }
    }
}
