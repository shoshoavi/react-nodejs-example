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
    }
}
