pipeline {
    agent any

    triggers {
        githubPush()
    }

    environment {
        EC2_USER = "ubuntu"
        EC2_IP = "3.109.5.131"
        PEM_PATH = "C:/Users/pd550/Downloads/web-key.pem"
        REMOTE_APP_DIR = "/home/ubuntu/myapp"
        SCP_EXE = "\"C:\\Program Files\\Git\\usr\\bin\\scp.exe\""
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
                "C:\\Program Files\\Git\\usr\\bin\\scp.exe" -o StrictHostKeyChecking=no -i "C:/Users/pd550/Downloads/web-key.pem" -r C:/ProgramData/Jenkins/.jenkins/workspace/dotnet-webapp/MyWebApp/out/* ubuntu@3.109.5.131:/home/ubuntu/myapp
                '''
            }
        }
        stage('Restart Remote App') {
    steps {
        bat '''
        ssh -o StrictHostKeyChecking=no -i "C:/Users/pd550/Downloads/web-key.pem" ubuntu@3.109.5.131 << EOF
        pkill dotnet || true
        nohup dotnet /home/ubuntu/myapp/MyWebApp.dll > /home/ubuntu/log.txt 2>&1 &
        EOF
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
    }
}
