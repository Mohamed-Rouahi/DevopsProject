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
        stage('AYA 3AD') {
            steps {
                script {
                    tool name: 'JAVA_HOME', type: 'jdk'
                    env.JAVA_HOME = tool 'JAVA_HOME'
                    sh "${env.JAVA_HOME}/bin/java -version"
                    sh 'mvn clean package'
                }
            }
        }
        stage("AYA 3AD YA SonarQube Analysis") {
    steps {
        withSonarQubeEnv('sonarQube') {
            script {
                def scannerHome = tool 'SonarQubeScanner'
                withEnv(["PATH+SCANNER=${scannerHome}/bin", "JAVAA_HOME=${env.JAVAA_HOME}"]) {
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
