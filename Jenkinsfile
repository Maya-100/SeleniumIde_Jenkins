
pipeline {
    agent any

    stages{
        stage('Checkout code') {
            //checkout the repository
            steps {
                git branch: 'main', url: 'https://github.com/Maya-100/SeleniumIde_Jenkins'
            }
        }
        stage('Set up .Net Core') {
            //install .Net
            steps {
                sh '''
                echo Installing .Net SDK 6.0
                /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
                brew install --cask dotnet-sdk6-0-132
                '''
            } 
        }
        stage('Restoring nuget packages') {
           //install dependencies
            steps {
                sh 'dotnet restore SeleniumIde.sln'
            } 
        }
        stage('Build') {
            //build
            steps {
                sh 'dotnet build SeleniumIde.sln -- configuration Release'
            }
        }
        stage('Run test') {
            //run test
            steps {
                sh 'dotnet test SeleniumIde.sln --logger "trx;LogFileName=TestResults.trx"'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/TestResults/*.trx', allowEmptyArchive: true
            step([
                $class: 'MSTestPublisher',
                testResultsFile: TestResults.trx
            ])
            
        }
    }
}

