pipeline {
    agent any

    environment {
        EC2_USER = "ubuntu"
        EC2_IP = "100.24.43.192"
        PEM_PATH = "C:/Users/pd550/Downloads/webappkey.pem"
        REMOTE_APP_DIR = "/home/ubuntu/myapp"
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
        stage('Copy to EC2') {
            steps {
                bat '''
                pscp -i "%PEM_PATH%" -r MyWebApp\\out\\* %EC2_USER%@%EC2_IP%:%REMOTE_APP_DIR%
                '''
            }
        }
        stage('Run on EC2') {
            steps {
                bat '''
                ssh -o StrictHostKeyChecking=no -i "%PEM_PATH%" %EC2_USER%@%EC2_IP% "pkill -f 'dotnet' || true && nohup dotnet %REMOTE_APP_DIR%/MyWebApp.dll > /dev/null 2>&1 &"
                '''
            }
        }
    }
}
