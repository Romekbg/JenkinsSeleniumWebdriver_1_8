pipeline {
    agent any

    stages {
        stage('Checkout code') {
            steps {
                git branch: 'main', url: 'https://github.com/Romekbg/JenkinsSeleniumWebdriver_1_8'
            }
        }
        stage('Set up .Net Core') {
            steps {
                bat '''
                echo Downloading .Net 6 SDK
                curl -L -o dotnet-install.ps1 https://dot.net/v1/dotnet-install.ps1
                echo Installing .Net 6 SDK
                powershell -ExecutionPolicy Bypass -File dotnet-install.ps1 -Version 6.0.132
                echo Verifying .Net SDK installation
                dotnet --version
                '''
            }
        }
        stage('Restore nuget packages') {
            steps {
                bat 'dotnet restore SeleniumBasicExercise.sln'
            }
        }
        stage('Build') {
            steps {
                bat 'dotnet build SeleniumBasicExercise.sln --configuration Release'
            }
        }
        stage('Run Tests TestProject1') {
            steps {
                bat 'dotnet test TestProject1/TestProject1.csproj --logger "trx;LogFileName=TestResults.trx"'
            }
        }

        stage('Run Tests TestProject2') {
            steps {
                bat 'dotnet test TestProject2/TestProject2.csproj --logger "trx;LogFileName=TestResults.trx"'
            }
        }

        stage('Run Tests TestProject3') {
            steps {
                bat 'dotnet test TestProject3/TestProject3.csproj --logger "trx;LogFileName=TestResults.trx"'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/TestResults/*.trx', allowEmptyArchive: true
            step([
                $class: 'MSTestPublisher',
                testResultsFile: '**/TestResults/*.trx'
            ])
        }
    }
}
