pipeline {
    agent any
    environment {
        strDockerImage = "y7u-dev/cicd-test:0.1"
    }
    stages {
        stage('Github Pull') {
            steps {
                git branch: 'main', url: 'https://github.com/y7u-dev/cicd-test.git'
            }
        }
        stage('Docker Image Build') {
            steps {
                script {
                    oDockerImage = docker.build(strDockerImage, "-f Dockerfile .")
                }
            }
        }
        stage('Docker Image Push') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'DockerHub-Credential') {
                        oDockerImage.push()
                    }
                }
            }
        }
        stage('Git clone end') {
            steps {
                sh 'touch cicd_test.txt'
                sh 'echo "git clone end" > cicd_test'
            }
        }
        stage('Deploy Server') {
            steps {
                sshagent(credentials: ['Deploy-Privatekey']) {
                    sh "scp -o StrictHostKeyChecking=no index.html ubuntu@3.38.185.241:/home/ubuntu/"
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@3.38.185.241 sudo cp /home/ubuntu/index.html /var/www/html"
                }
            }
        }
    }
}