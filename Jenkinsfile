#!groovy

pipeline{
    agent any
    environment{
        HOME = "${env.WORKSPACE}"
    }

    stages{
        stage('Docker environment'){
            agent{
                docker{
                    image 'python:3.11-slim'
                    reuseNode true
                }

            }
            steps{
                sh"""
                pip install -r requirements.txt
                """
            }
        }
        stage('Build'){
            steps{
                sh"""
                docker build -t carlosvicente1979/nif-validator .
                """
            }
        }
        stage('Deliver') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'docker-id',
                    passwordVariable: 'passwd', 
                    usernameVariable: 'username')]) {
                        sh"""
                        printenv
                        docker build -t ${username}/nif-validator .
                        docker login -u ${username} -p ${passwd}
                        docker push ${username}/nif-validator
                        """
               }
            }
        }
    }
}