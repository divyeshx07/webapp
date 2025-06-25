pipeline {
    agent any

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
