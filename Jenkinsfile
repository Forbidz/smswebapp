pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('sonarqube-token')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    script {
                        def scannerHome = tool name: 'SonarScanner_for_MSBuild', type: 'hudson.plugins.sonar.MsBuildSQRunnerInstallation'

                        // Paso begin
                        bat """
                            "${scannerHome}\\SonarScanner.MSBuild.exe" begin ^
                            /k:"smswebapp" ^
                            /d:sonar.host.url="http://localhost:9000" ^
                            /d:sonar.login=${env.SONAR_TOKEN}
                        """

                        // Compilación
                        bat "dotnet build"

                        // Paso end
                        bat """
                            "${scannerHome}\\SonarScanner.MSBuild.exe" end ^
                            /d:sonar.login=${env.SONAR_TOKEN}
                        """
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Análisis SonarQube completado con éxito.'
        }
        failure {
            echo 'El análisis falló. Revisa los logs.'
        }
    }
}

