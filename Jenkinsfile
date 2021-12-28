pipeline {
    environment {
        dockerimagename = "drewzydrew/node-web-app"
        dockerImage = ""
    }

    agent any

    stages {
        stage('Build Image') {
            steps {
                script {
                    dockerImage = docker.build("drewzydrew/node-web-app:${env.BUILD_ID}")
                }
            }
        }

        stage('Test image') {
            steps {
                echo 'testing application'
                                echo 'testing application'
            }
        }

        stage('Push image to DockerHub') {
            environment {
                registryCredentials = 'dockerhublogin'
            }
            steps {
                script {
                    docker.withRegistry('', registryCredentials) {
                        dockerImage.push()
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo 'testing application'
            }
        }
    }
}
