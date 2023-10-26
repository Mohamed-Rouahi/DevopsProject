pipeline {
    agent any
    stages {
        stage('Set Java Version') {
            steps {
                script {
                    // Set the default JAVA_HOME to Java 8
                    tool name: 'JAVAA_HOME', type: 'jdk'
                }
            }
        }
        stage('Checkout Backend Repo') {
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
        stage('BUILD Backend') {
            steps {
                // Use Java 8 for this stage
                withEnv(["JAVA_HOME=${tool name: 'JAVAA_HOME', type: 'jdk'}"]) {
                    sh 'mvn clean package'
                }
            }
        }
        stage('COMPILE Backend') {
            steps {
                // Use the default Java 8 for this stage
                sh 'mvn compile'
            }
        }
        stage("SonarQube Analysis") {
            steps {
                // Set Java 11 for this stage
                tool name: 'JAVA_HOME', type: 'jdk'
                withEnv(["JAVA_HOME=${tool name: 'JAVA_HOME', type: 'jdk'}"]) {
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
        stage('Checkout Frontend Repo') {
            steps {
                script {
                    // Checkout the frontend repository
                    checkout([
                        $class: 'GitSCM',
                        branches: [[name: 'master']], 
                        userRemoteConfigs: [[url: 'https://github.com/Mohamed-Rouahi/Project-devops-frontend.git']]
                    ])
                }
            }
        }
        stage('Build Frontend') {
    steps {
        // Set the Node.js tool defined in Jenkins configuration
        script {
            def nodeJSHome = tool name: 'nodejs' // Use the correct tool name
            env.PATH = "${nodeJSHome}/bin:${env.PATH}"
        }
        // Now you can run 'npm install' and 'ng build'
        sh 'npm install'
        sh 'npm run ng build'
    }
}
        stage('Login to Docker Registry') {
            steps {
                script {
                    // Utilisez votre nom d'utilisateur et mot de passe Docker Hub
                    withCredentials([string(credentialsId: 'docker', usernameVariable: 'medrouahi', passwordVariable: 'ertydfgh98')]) {
                        sh "docker login -u $medrouahi -p $ertydfgh98"
                    }
                }
            }
        stage('Build and Push Docker Images') {
            steps {
                script {
                    def backendImage = docker.build('medrouahi/devopsBackend', '-f DevopsProject/Dockerfile .')
                    backendImage.push()
                    
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
