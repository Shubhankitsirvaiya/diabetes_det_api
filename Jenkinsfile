pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Echo') {
            steps {
                echo "Jenkins setup works!"
            }
        }
    }
}
