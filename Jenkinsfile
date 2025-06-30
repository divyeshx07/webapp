pipeline {
    agent any

    environment {
        EC2_USER = "ubuntu"
        EC2_IP = "65.0.169.36"
        PEM_PATH = "C:/Users/pd550/Downloads/web-key.pem"
        REMOTE_APP_DIR = "/home/ubuntu/myapp"
        SCP_EXE = "\"C:\\Program Files\\Git\\usr\\bin\\scp.exe\"" // escaped for Windows
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
                    bat 'if exist out (rmdir /S /Q out)'
                    bat 'dotnet publish -c Release -o out'
                }
            }
        }

        stage('Copy to EC2') {
    steps {
        bat '''
        "C:\\Program Files\\Git\\usr\\bin\\scp.exe" -o StrictHostKeyChecking=no -i "C:/Users/pd550/Downloads/web-key.pem" -r C:/ProgramData/Jenkins/.jenkins/workspace/dotnet-webapp/MyWebApp/out/* ubuntu@65.0.169.36:/home/ubuntu/myapp
        '''
    }
}

        stage('Run') {
            steps {
                dir('MyWebApp') {
                    bat 'taskkill /F /IM dotnet.exe || exit 0'
                    bat 'start /B dotnet out\\MyWebApp.dll'
                }
            }
        }
    } // <-- Make sure this closes the stages block
} // <-- This closes the pipeline block
