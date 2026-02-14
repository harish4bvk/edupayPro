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
        }        stage('Ask for Docker IP') {
            steps {
                script {
                    def dockerIp = input(
                        id: 'DockerIP',
                        message: 'Enter the Docker Server IP:',
                        parameters: [
                            string(name: 'DOCKER_IP', description: 'Provide the Docker server IP address')
                        ]
                    )
                    echo "Docker IP entered: ${dockerIp}"
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh """
                    scp -r . ec2-user@${dockerIp}/home/ec2-user/app
                    ssh ec2-user@${dockerIp} "cd /home/ec2-user/app && docker build -t myapp:latest ."
                    """
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    sh """
                    ssh ec2-user@${dockerIp} "docker run -d --name myapp_container -p 8080:80 myapp:latest"
                    """
                }
            }
        }
    }
}
