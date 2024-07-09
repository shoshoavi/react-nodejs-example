pipeline {
    agent any
    enviroment {
//        SERVER_CREDENTIALS = credentials('Dockerhub_user')
    }
    stages {
        stage('building image') {
            steps {
                echo 'building image from Dockerfile'
                sh "docker build -t shoshoavi/demo-app:1.0.0 ."
            }
        }
        stage('login and push the image') {
            steps {
                echo 'pushing the image to the repository'
                withCredentials([
                        usernamePassword(credentialsId: 'Dockerhub_user', passwordVariable: 'PWD', usernameVariable: 'USER')
                    ]) {
                    sh "echo ${PWD} | docker login -u ${USER} --password-stdin"
                    sh "docker push shoshoavi/demo-app:1.0.0"
                }
            }
        }
        stage('deploying the image') {
            steps {
                script {
                    def dockerCmd = 'docker run -d -p 3000:80 shoshoavi/demo-app:1.0.0'
                    echo 'deploying the image to the remote ec2 machine'
                    sshagent(['ec2-credentials']) {
                      sh "ssh -o StrictHostKeyChecking=no ec2-user@44.214.104.210 ${dockerCmd}"
                    }
                }
            }
        }
    }
}
