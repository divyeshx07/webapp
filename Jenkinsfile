pipeline {
    agent any

    environment {
        EC2_USER = "ubuntu"
        EC2_IP = "65.0.169.36"
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
                    // Clean previous publish output first (optional but safe)
                    bat 'if exist out (rmdir /S /Q out)'

                    // Now publish to a clean 'out' folder
                    bat 'dotnet publish -c Release -o out'
                }
            }
        }

        stage('Copy to EC2') {
            steps {
              bat '''
                 scp -i "%PEM_PATH%" -r MyWebApp\\out\\* %EC2_USER%@%65.0.169.36%:%REMOTE_APP_DIR%
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
