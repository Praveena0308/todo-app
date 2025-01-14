pipeline {
    agent any

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Praveena0308/to-do-app.git'
            }
        }
        stage('Docker Build') {
            steps {
                script {
                    // Build Docker image with Dockerfile located in 'backend' directory
                    sh 'docker build -t todoapp:latest -f backend/Dockerfile backend'
                }
            }
        }
        stage('Docker Push') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'Docker-cred') {
                        sh 'docker tag todoapp:latest prave987/todoapp:latest'
                        sh 'docker push prave987/todoapp:latest'
                    }
                }
            }
        }
        stage('Deploy on EC2') {
            steps {
                script {
                    echo 'Deploying Docker image to EC2...'
                    def dockerCmd = "docker run -p 8081:8081 -d prave987/todoapp:latest"
                    sshagent(['ec2-server-key']) {
                        sh "ssh -o StrictHostKeyChecking=no ec2-user@3.123.25.61 '${dockerCmd}'"
                    }
                }
            }
        }
    }
}
