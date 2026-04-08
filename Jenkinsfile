pipeline {
    agent any
    stages {
        stage('Github Pull') {
            steps {
                git branch: 'main', url: 'https://github.com/y7u-dev/cicd-test.git'
            }
        }                                          
        stage('Git clone end') {
            steps {
                sh 'touch cicd_test.txt'
                sh 'echo "git clone end" > cicd_test'  
            }
        }
        stage('Deploy Server') {       // ← ② 오타 수정
            steps {                    // ← ① steps 추가
                sshagent(credentials: ['Deploy-Privatekey']) {
                    sh "scp -o StrictHostKeyChecking=no index.html ubuntu@3.38.185.241:/home/ubuntu/"
		            sh "ssh -o StrictHostKeyChecking=no ubuntu@3.38.185.241 sudo cp /home/ubuntu/index.html /var/www/html"
                }
            }
        }
    }
}