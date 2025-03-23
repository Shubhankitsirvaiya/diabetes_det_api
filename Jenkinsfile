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
                    // Fix for Git safe directory issue
                    sh "git config --global --add safe.directory /var/jenkins_home/workspace/${env.JOB_NAME}"

                    // Get short commit hash
                    def commitHash = sh(script: "git rev-parse --short HEAD", returnStdout: true).trim()

                    // Build and tag Docker image
                    sh "docker build -t $IMAGE_NAME:$commitHash ."
                    sh "docker tag $IMAGE_NAME:$commitHash $IMAGE_NAME:latest"

                    // Save commitHash to env for use in next stage
                    env.IMAGE_TAG = commitHash
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    // Login to Docker
                    sh "echo $DOCKER_HUB_CREDENTIALS_PSW | docker login -u $DOCKER_HUB_CREDENTIALS_USR --password-stdin"

                    // Push both tags
                    sh "docker push $IMAGE_NAME:$IMAGE_TAG"
                    sh "docker push $IMAGE_NAME:latest"
                }
            }
        }
    }
}
