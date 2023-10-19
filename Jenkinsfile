pipeline {
    agent any // Define the agent for the pipeline (e.g., any available agent)

    stages {
        stage('Checkout') {
            steps {
                script {
                    checkout([
                        $class: 'GitSCM',
                        branches: [[name: '*/master']], // You can specify the branch you want to build here.
                        userRemoteConfigs: [[url: 'https://github.com/Mohamed-Rouahi/DevopsProject.git']]
                    ])
                }
            }
        }
        stage('build') {
            steps {               
                sh 'mvn clean install'
            }
        }
    }
}
