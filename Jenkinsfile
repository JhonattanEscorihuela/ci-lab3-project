pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                script {
                    echo "Building application for branch ${env.BRANCH_NAME}"
                }
            }
        }
        stage('Test') {
            steps {
                echo "Running tests for branch ${env.BRANCH_NAME}"
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    def imageName = (env.BRANCH_NAME == 'main') ? 'nodemain:v1.0' : 'nodedev:v1.0'
                    // Usar 'bat' para ejecutar comandos en Windows
                    bat """
                    docker build -t ${imageName} .
                    """
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    def port = (env.BRANCH_NAME == 'main') ? '3000' : '3001'
                    def containerName = (env.BRANCH_NAME == 'main') ? 'node_main' : 'node_dev'

                    // Usar 'bat' para los comandos de despliegue
                    bat """
                    docker ps -q --filter "name=${containerName}" | for /F "tokens=*" %i in ('more') do docker rm -f %i
                    docker run -d --name ${containerName} -p ${port}:3000 nodemain:v1.0
                    """
                }
            }
        }
    }
}
