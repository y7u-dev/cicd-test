pipeline {
    agent any
    environment {                                         // ← ① 오타 수정
        strDockerImage = "y7u-dev/cicd-test:0.1"         // ← ② 변수명 통일
    }
    stages {
        stage('Github Pull') {
            steps {                                       // ← ③ 구조 수정
                git branch: 'main', url: 'https://github.com/y7u-dev/cicd-test.git'
            }
        }                     
        stage('Docker Image Build') {
            steps {
                script {
                    oDockerImage = docker.build(strDockerImage, "-f Dockerfile")  // ← ④⑤ 수정
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