pipeline {
    agent any

    environment {
        // Replace with your GitHub repo URL
        GIT_REPO = 'https://github.com/your-username/your-repo.git'
        BRANCH   = 'main'
    }

    stages {
        stage('Checkout') {
            steps {
                // Pull source code from GitHub
                git branch: "${BRANCH}", url: "${GIT_REPO}" credentialsId: 'github'

            }
        }

        stage('List Files') {
            steps {
                // Just to verify code is pulled
                sh 'ls -l'
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed.'
        }
    }
}
