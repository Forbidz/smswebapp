pipeline {
    agent any

    environment {
        DOTNET_CLI_HOME = "C:\\Program Files\\dotnet"
        SONAR_SCANNER = "Sonar-scanner" // Nombre que pusiste en Jenkins
        SONARQUBE = "SonarQube" // Nombre de la instalación de SonarQube
        SONAR_TOKEN = credentials('sonarqube-token') // ID de la credencial en Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('SonarQube Begin') {
            steps {
                withSonarQubeEnv("${SONARQUBE}") {
                    bat """
                        ${SONAR_SCANNER}\\SonarScanner.MSBuild.exe begin /k:"smswebapp" /d:sonar.login=${SONAR_TOKEN}
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
                    ${SONAR_SCANNER}\\SonarScanner.MSBuild.exe end /d:sonar.login=${SONAR_TOKEN}
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
            echo 'Build, test, publish, and SonarQube analysis successful!'
        }
    }
}
