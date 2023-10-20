pipeline {
    agent any
    stages {
        stage('Set Java Version') {
            steps {
                script {
                    // Set the default JAVA_HOME to Java 8
                    tool name: 'JAVA_HOME', type: 'jdk'
                    env.JAVA_HOME = tool 'JAVA_HOME'
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
        stage('AYA 3AD') {
            steps {
                // Use Java 8 for this stage
                withEnv(["JAVA_HOME=${env.JAVA_HOME}"]) {
                    sh 'mvn clean package'
                }
            }
        }

        stage("AYA 3AD YA SonarQube Analysis") {
            steps {
                // Set Java 11 for this stage
                tool name: 'JAVAA_HOME', type: 'jdk'
                env.JAVA_HOME = tool 'JAVAA_HOME'
                withSonarQubeEnv('sonarQube') {
                    script {
                        def scannerHome = tool 'SonarQubeScanner'
                        withEnv(["PATH+SCANNER=${scannerHome}/bin", "JAVA_HOME=${env.JAVA_HOME}"]) {
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
