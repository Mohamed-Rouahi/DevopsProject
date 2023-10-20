pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                script {
                    // Checkout your source code from the repository.
                    checkout([
                        $class: 'GitSCM',
                        branches: [[name: '*/master']], 
                        userRemoteConfigs: [[url: 'https://github.com/Mohamed-Rouahi/DevopsProject.git']]
                    ])
                }
            }
        }

        stage('Clean') {
            steps {
                sh 'mvn clean'
            }
            post {
                success {
                    emailext(
                        subject: "Success: Clean Stage Completed",
                        body: "The clean stage was successful.",
                        to: "recipient@example.com"
                    )
                }
                failure {
                    emailext(
                        subject: "Failure: Clean Stage Failed",
                        body: "The clean stage has failed.",
                        to: "recipient@example.com"
                    )
                }
            }
        }

        stage('COMPILE') {
            steps {
                sh 'mvn compile'
            }
        }

        stage("SonarQube analysis") {
            steps {
                withSonarQubeEnv('sonarQube') {
                    script {
                        def scannerHome = tool 'SonarQubeScanner'
                        withEnv(["PATH+SCANNER=${scannerHome}/bin"]) {
                            sh '''
                                mvn sonar:sonar \
                                    -Dsonar.java.binaries=target/classes
                            '''
                        }
                    }
                }
            }
            post {
                success {
                    emailext(
                        subject: "Success: SonarQube Analysis Completed",
                        body: "SonarQube analysis was successful.",
                        to: "mohamed.rouahi@esprit.tn"
                    )
                }
                failure {
                    emailext(
                        subject: "Failure: SonarQube Analysis Failed",
                        body: "SonarQube analysis has failed.",
                        to: "mohamed.rouahi@esprit.tn"
                    )
                }
            }
        }
    }
}
