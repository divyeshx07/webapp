pipeline {
    agent any

    stages {
        stage('Restore') {
            steps {
                bat 'dotnet restore'
            }
        }
        stage('Build') {
            steps {
                bat 'dotnet build --configuration Release'
            }
        }
        stage('Publish') {
            steps {
                bat 'dotnet publish -c Release -o out'
            }
        }
        stage('Run') {
            steps {
                bat 'taskkill /F /IM dotnet.exe || exit 0'
                bat 'start /B dotnet out\\*.dll'
            }
        }
    }
}
