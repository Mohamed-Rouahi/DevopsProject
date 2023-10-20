pipeline {
    agent any
    stages {
        stage('Set Java Version') {
            steps {
                script {
                    tool name: 'JAVA_HOME', type: 'jdk'
                    env.JAVA_HOME = tool 'JAVA_HOME'
                    sh "${env.JAVA_HOME}/bin/java -version"
                }
            }
        }
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
                sh 'mvn clean package'
            }
        }
        stage('Compile') {
            steps {
                sh 'mvn compile'
            }
        }
        stage("SonarQube Analysis") {
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
