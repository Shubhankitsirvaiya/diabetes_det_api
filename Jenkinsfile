pipeline {
    agent any
    environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub-creds')
        IMAGE_NAME = "sirvaiys/fastapi-diabetes"
    }
    stages {
        stage('Build Image') {
            steps {
                script {
                    def commitHash = env.GIT_COMMIT.take(7)
                    sh "docker build -t $IMAGE_NAME:$commitHash ."
                    sh "docker tag $IMAGE_NAME:$commitHash $IMAGE_NAME:latest"
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                script {
                    def commitHash = env.GIT_COMMIT.take(7)
                    sh "echo $DOCKER_HUB_CREDENTIALS_PSW | docker login -u $DOCKER_HUB_CREDENTIALS_USR --password-stdin"
                    sh "docker push $IMAGE_NAME:$commitHash"
                    sh "docker push $IMAGE_NAME:latest"
                }
            }
        }
    }
}
