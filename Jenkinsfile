import java.text.SimpleDateFormat
def TODAY = (new SimpleDateFormat("yyyyMMdd")).format(new Date())
pipeline {
    agent any
    environment {
        strDockerTag = "${TODAY}_${BUILD_ID}"
        strDockerImage = "gnwowne/cicd-test:${strDockerTag}"
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
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@3.38.185.241 docker container rm -f sampleweb"
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@3.38.185.241 docker run -d -p 80:80 --name sampleweb ${strDockerImage}"
                }
            }
        }
    }
}