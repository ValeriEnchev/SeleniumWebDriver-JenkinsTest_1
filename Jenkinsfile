pipeline {
    agent any
    stages {
        stage('Checkout code') {
            steps {
                // Checkout code from Github and specify the branch
                git branch: 'main', url: 'https://github.com/ValeriEnchev/SeleniumWebDriver-JenkinsTest_1.git'
            }
        } 
        stage('Set up dotNet Core') {
            steps {
				bat '''
					echo Downloading dotNet 6 SDK installer
					curl -o dotnet-sdk-6.0.136-win-x86.exe https://builds.dotnet.microsoft.com/dotnet/Sdk/6.0.136/dotnet-sdk-6.0.136-win-x86.exe
					echo Installing dotnet-sdk-6.0.136-win-x86.exe
					dotnet-sdk-6.0.136-win-x86.exe /quiet /norestart
				'''
            }
        } 
		stage('Restore dependencies') {
            steps {
                bat 'dotnet restore SeleniumBasicExercise.sln'
            }
		}
        stage('Build') {
            steps {
                bat 'dotnet build SeleniumBasicExercise.sln --configuration Release'
            }
		}
        stage('Run TestProject1 Tests') {
            steps {
                bat 'dotnet test TestProject1/TestProject1.csproj --logger "trx;LogFileName=TestResult.trx"'
            }
		}
		stage('Run TestProject2 Tests') {
            steps {
                bat 'dotnet test TestProject2/TestProject2.csproj --logger "trx;LogFileName=TestResult.trx"'
            }
		}
		stage('Run TestProject3 Tests') {
            steps {
                bat 'dotnet test TestProject3/TestProject3.csproj --logger "trx;LogFileName=TestResult.trx"'
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