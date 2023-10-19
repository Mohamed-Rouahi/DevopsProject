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
        stage('Send Email') {
            steps {
                mail bcc: 'sffsfs', body: 'test test test', cc: 'sffs', from: '', replyTo: '', subject: 'test sending mail', to: 'mohamed.rouahi@esprit.tn'
            }
        }
        stage('build') {
            steps {               
                sh 'mvn clean compile'
            }
        }
        stage("SonarQube analysis") {
            agent any
            steps {
              withSonarQubeEnv('sonarQube') {
                sh 'mvn sonar:sonar'
              } 
            }
          }
    }
}
