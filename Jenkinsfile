pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub-creds')
        IMAGE_NAME = "sirvaiys/fastapi-diabetes"
    }

    stages {
        stage('Clone Repo') {
            steps {
                script {
                    sh "rm -rf *"
                    sh "git clone https://github.com/Shubhankitsirvaiya/diabetes_det_api.git ."
                    sh "git config --global --add safe.directory /var/jenkins_home/workspace/${env.JOB_NAME}"
                }
            }
        }

        stage('Build Image') {
            steps {
                script {
                    def commitHash = sh(script: 'git rev-parse --short HEAD', returnStdout: true).trim()
                    env.COMMIT_HASH = commitHash
                    sh "docker build -t $IMAGE_NAME:$COMMIT_HASH ."
                    sh "docker tag $IMAGE_NAME:$COMMIT_HASH $IMAGE_NAME:latest"
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    sh "echo $DOCKER_HUB_CREDENTIALS_PSW | docker login -u $DOCKER_HUB_CREDENTIALS_USR --password-stdin"
                    sh "docker push $IMAGE_NAME:$COMMIT_HASH"
                    sh "docker push $IMAGE_NAME:latest"
                }
            }
        }
    }
}
