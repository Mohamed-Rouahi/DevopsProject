pipeline {
        stage('Checkout') {
            steps {
                // Checkout your source code from the repository.
                script {
                    https://github.com/Mohamed-Rouahi/DevopsProject.git
                    checkout scm
                }
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/master']], // You can specify the branch you want to build here.
                    userRemoteConfigs: [[url: 'https://github.com/Mohamed-Rouahi/DevopsProject.git']]
                ])
            }
        }

        stage('Unit Tests') {
            steps {              
                 sh 'mvn clean'
                
            }
        }

        
    }



