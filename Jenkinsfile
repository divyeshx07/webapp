pipeline {
    agent any

    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/Divyeshx07/webapp.git'
            }
        }
        stage('Restore') {
            steps {
                sh 'dotnet restore'
            }
        }
        stage('Build') {
            steps {
                sh 'dotnet build --configuration Release'
            }
        }
        stage('Publish') {
            steps {
                sh 'dotnet publish -c Release -o out'
            }
        }
        stage('Run') {
            steps {
                sh 'pkill dotnet || true'
                sh 'nohup dotnet out/*.dll > output.log 2>&1 &'
            }
        }
    }
}
