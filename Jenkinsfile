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
        }

      stage('COMPILE') {
            steps {
                sh 'mvn compile'
            }
        }
           stage('Email notification') {
            steps {
                mail bcc: '', body: '''this is a Jenkins email alerts linked with GitHub 
                    test
                    thank you
                    Azza KOUKA''', cc: '', from: '', replyTo: '', subject: 'Jenkins notification', to: 'azza.kouka@esprit.tn'
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
}
