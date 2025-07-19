pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('sonarqube-token') // ID que registraste
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('SonarQube Begin') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    bat """
                        SonarScanner_for_MSBuild\\SonarScanner.MSBuild.exe begin ^
                        /k:"smswebapp" ^
                        /d:sonar.host.url="http://localhost:9000" ^
                        /d:sonar.login=${SONAR_TOKEN}
                    """
                }
            }
        }

        stage('Build') {
            steps {
                bat "dotnet restore"
                bat "dotnet build --configuration Release"
            }
        }

        stage('Test') {
            steps {
                bat "dotnet test --no-restore --configuration Release"
            }
        }

        stage('SonarQube End') {
            steps {
                bat """
                    SonarScanner_for_MSBuild\\SonarScanner.MSBuild.exe end ^
                    /d:sonar.login=${SONAR_TOKEN}
                """
            }
        }

        stage('Publish') {
            steps {
                bat "dotnet publish --no-restore --configuration Release --output .\\publish"
            }
        }
    }

    post {
        success {
            echo 'Build + test + SonarQube OK!'
        }
    }
}
