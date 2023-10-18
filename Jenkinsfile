pipeline {
    agent any // Define the agent for the pipeline (e.g., any available agent)

    stages {
        stage('Checkout and Build') {
            steps {
                script {
                    // Checkout your source code from the repository.
                    checkout([
                        $class: 'GitSCM',
                        branches: [[name: '*/master']], // You can specify the branch you want to build here.
                        userRemoteConfigs: [[url: 'https://github.com/Mohamed-Rouahi/DevopsProject.git']]
                    ])

                    // Run 'mvn clean install' to clean and build the project
                    sh 'mvn clean install'
                }
            }
        }

        // Add more stages for additional build, test, or deployment steps
    }
}
