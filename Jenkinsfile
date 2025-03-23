pipeline {
    agent any

    options {
    skipDefaultCheckout()
    }

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub-creds')
        IMAGE_NAME = "sirvaiys/fastapi-diabetes"
    }

    stages {
        stage('Build Image') {
            steps {
                script {
                    def commitHash = sh(script: "git rev-parse --short HEAD", returnStdout: true).trim()
                    sh "docker build -t $IMAGE_NAME:$commitHash ."
                    sh "docker tag $IMAGE_NAME:$commitHash $IMAGE_NAME:latest"
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    def commitHash = sh(script: "git rev-parse --short HEAD", returnStdout: true).trim()
                    sh "echo $DOCKER_HUB_CREDENTIALS_PSW | docker login -u $DOCKER_HUB_CREDENTIALS_USR --password-stdin"
                    sh "docker push $IMAGE_NAME:$commitHash"
                    sh "docker push $IMAGE_NAME:latest"
                }
            }
        }
    }
}
