pipeline {
    agent any

    stages {
        stage('Restore') {
            steps {
                dir('webapp') {
                    bat 'dotnet restore'
                }
            }
        }
        stage('Build') {
            steps {
                dir('webapp') {
                    bat 'dotnet build --configuration Release'
                }
            }
        }
        stage('Publish') {
            steps {
                dir('webapp') {
                    bat 'dotnet publish -c Release -o out'
                }
            }
        }
        stage('Run') {
            steps {
                dir('webapp') {
                    bat 'taskkill /F /IM dotnet.exe || exit 0'
                    bat 'start /B dotnet out\\*.dll'
                }
            }
        }
    }
}
