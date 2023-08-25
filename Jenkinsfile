pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE_TAG = "ademboujnah/nodejs-app:latest"
    }
    
    stages {
        stage('Build') {
            steps {
                // Build the Node.js app in a Docker container
                sh 'docker build -t $DOCKER_IMAGE_TAG .'
            }
        }
         stage('Code Analysis') {
            steps {
                script {
                    def scannerHome = tool 'SonarQubeScanner'
                    withSonarQubeEnv('SonarQube') {
                        // Run SonarQube code analysis
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }
        
        stage('Push to Docker Hub') {
            steps {
                // Authenticate with Docker Hub using credentials
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials-id', passwordVariable: 'DOCKERHUB_PASSWORD', usernameVariable: 'DOCKERHUB_USERNAME')]) {
                    sh "docker login -u $DOCKERHUB_USERNAME -p $DOCKERHUB_PASSWORD"
                    sh "docker push $DOCKER_IMAGE_TAG"
                }
            }
        }
    }
}
