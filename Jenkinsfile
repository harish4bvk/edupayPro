pipeline {
    agent any

    environment {
        GIT_REPO = 'https://github.com/harish4bvk/edupayPro.git'
        BRANCH   = 'main'
        DOCKER_IP = ''   // declare globally
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: "${BRANCH}", url: "${GIT_REPO}", credentialsId: 'github'
                sh 'echo "Git checkout complete"'
            }
        }

        stage('Ask for Docker IP') {
            steps {
                script {
                    def userInput = input(
                        id: 'DockerIP',
                        message: 'Enter the Docker Server IP:',
                        parameters: [
                            string(name: 'DOCKER_IP', defaultValue: '13.232.84.226', description: 'Provide the Docker server IP address')
                        ]
                    )
                    env.DOCKER_IP = userInput
                    echo "Docker IP entered: ${env.DOCKER_IP}"
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh """
                    scp -r . ec2-user@${env.DOCKER_IP}:/home/ec2-user/app
                    ssh ec2-user@${env.DOCKER_IP} "cd /home/ec2-user/app && docker build -t myapp:latest ."
                    """
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    sh """
                    ssh ec2-user@${env.DOCKER_IP} "docker run -d --name myapp_container -p 8080:80 myapp:latest"
                    """
                }
            }
        }
    }
}
