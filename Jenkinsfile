pipeline {
    agent any

    environment {
        REGISTRY = "avanthikha" // Use the same DockerHub username
        IMAGE_NAME = "webapp"
        TAG = "latest"
        // WORKDIR = "/root/kec" // Change the folder path based on where your Dockerfile and docker-compose.yml reside
        WORKDIR = "/var/lib/jenkins/workspace/tasj2"
    }

    stages {
        stage('Build & Push Docker Image') {
            steps {
                dir("$WORKDIR") {  
                    sh 'docker build -t $REGISTRY/$IMAGE_NAME:$TAG .'
                    withDockerRegistry([credentialsId: 'docker-hub-creds', url: '']) {
                        sh 'docker push $REGISTRY/$IMAGE_NAME:$TAG'
                    }
                }
            }
        }

        stage('Deploy Application') {
            steps {
                dir("$WORKDIR") {
                    sh 'docker-compose up -d'
                }
            }
        }
    }

    post {
        success {
            echo 'Build, Push & Deploy Successful!'
        }
        failure {
            echo 'Pipeline Failed. Check logs!'
        }
    }
}
