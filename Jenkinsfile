pipeline {
    agent any

    environment {
        IMAGE_NAME = "mywebapp"
        CONTAINER_NAME = "mywebapp-container"
    }

    stages {
        stage('Restore') {
            steps {
                dir('MyWebApp') {
                    bat 'dotnet restore'
                }
            }
        }

        stage('Build') {
            steps {
                dir('MyWebApp') {
                    bat 'dotnet build --configuration Release'
                }
            }
        }

        stage('Publish') {
            steps {
                dir('MyWebApp') {
                    bat 'dotnet publish -c Release -o out'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dir('MyWebApp') {
                        bat "docker build -t ${IMAGE_NAME} ."
                    }
                }
            }
        }

        stage('Stop Old Container') {
            steps {
                script {
                    // Stop and remove old container if exists
                    bat "docker stop ${CONTAINER_NAME} || exit 0"
                    bat "docker rm ${CONTAINER_NAME} || exit 0"
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                bat "docker run -d -p 9090:5000 --name ${CONTAINER_NAME} ${IMAGE_NAME}"
            }
        }
    }
}
