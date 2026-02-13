pipeline {
    agent any

    environment {
        GIT_REPO = 'https://github.com/harish4bvk/edupayPro.git'
        BRANCH   = 'main'
    }

    stages {
        stage('Checkout') {
            steps {
                // Correct usage: commas separate arguments
                git branch: "${BRANCH}", url: "${GIT_REPO}", credentialsId: 'github'
            }
        }

      stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t edupaypro:0.1 ."
                }
            }
        } 
    } 

    post {
        always {
            echo 'Pipeline finished.'
        }
    }
}
