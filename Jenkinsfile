
pipeline {
    agent any

    stages{
        stageCheckout code {
            //checkout the repository
            steps {
                git branch: 'main', url: 'https://github.com/Maya-100/SeleniumIde_Jenkins'
            }
        stage('Set up .Net Core') {
            //install .Net
            steps {
                bat '''
                echo Installing .Net SDK 6.0
                curl -l -o dotnet-sdk-6.0.132-osx-arm64.pkg https://download.visualstudio.microsoft.com/download/pr/e3da35eb-fa30-4668-be20-8e40c53c580b/506b1cfe85be2a73f772e4089e7d95d2/dotnet-sdk-6.0.132-osx-arm64.pkg
                echo installing dotnet-sdk-6.0.132-osx-arm64.pkg
                dotnet-sdk-6.0.132-osx-arm64.pkg/quiet/norestart
                '''
            } 
        }
        stage('Restoring nuget packages {
           //install dependencies
            steps {
                bat 'dotnet restore SeleniumIde.sln'
            } 
        }
        stage('Build') {
            //build
            steps {
                bat 'dotnet build SeleniumIde.sln -- configuration Release'
            }
        }
        stage('Run test') {
            //run test
            steps {
                bat 'dotnet test SeleniumIde.sln --logger "trx;LogFileName=TestResults.trx"'
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


