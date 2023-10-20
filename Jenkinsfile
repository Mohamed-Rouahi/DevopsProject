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

        stage('build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('COMPILE') {
            steps {
                sh 'mvn test'
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
        }
    }

    post {
        success {
            mail to: 'mohamed.rouahi@esprit.tn',
                 subject: 'Jenkins Notification: Success',
                 body: '''This is a Jenkins email alert linked with GitHub.
                    Test
                    Thank you
                    Mohamed Rouahi'''
        }

        failure {
            mail to: 'mohamed.rouahi@esprit.tn',
                 subject: 'Jenkins Notification: Failure',
                 body: '''This is a Jenkins email alert linked with GitHub.
                    Test
                    Thank you
                    Mohamed Rouahi'''
        }
    }
}
