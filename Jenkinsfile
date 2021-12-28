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
                sshagent(['ubuntussh']) {
                    sh "ssh ubuntu@52.4.33.41 \"docker container run --detach --rm --name test drewzydrew/node-web-app:${env.BUILD_ID}\""
                    sh "ssh ubuntu@52.4.33.41 \"docker stop test\""
                }
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
                sshagent(['ubuntussh']) {
                    sh "ssh ubuntu@52.4.33.41 \"kubectl set image deployments/node-web-app node-web-app=drewzydrew/node-web-app:${env.BUILD_ID}\""
                }
            }
        }
    }
}
