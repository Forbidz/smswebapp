pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('sonarqube-token')
    }

    stages {
        stage('SCM') {
            steps {
                checkout scm
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    script {
                        def scannerHome = tool 'SonarScanner_for_MSBuild'
                        bat """
                            ${scannerHome}\\SonarScanner.MSBuild.exe begin ^
                            /k:"smswebapp" ^
                            /d:sonar.host.url="http://localhost:9000" ^
                            /d:sonar.login=${SONAR_TOKEN}
                        """
                        bat "dotnet build"
                        bat """
                            ${scannerHome}\\SonarScanner.MSBuild.exe end ^
                            /d:sonar.login=${SONAR_TOKEN}
                        """
                    }
                }
            }
        }
    }
}
