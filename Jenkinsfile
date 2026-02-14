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
                sh '''
                echo git checkout 
                '''    
            }
        }
        stage('Build Docker Image') {
            steps {
                // Build image on Docker Server via SSH
                sh '''
                scp -r . ec2-user@43.205.126.252:/home/ec2-user/app
                ssh ec2-user@43.205.126.252 "cd /home/ec2-user/app && docker build -t myapp:latest ."
                '''
            }
        }

        stage('Run Container') {
            steps {
                // Run container on Docker Server
                sh '''
                ssh ec2-user@43.205.126.252 "docker run -d --name myapp_container -p 8080:8080 myapp:latest"
                '''
            }
        }
    } 

    post {
        always {
            echo 'Pipeline finished.'
        }
    }
}
