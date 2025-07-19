node {
  stage('SCM') {
    checkout scm
  }
  stage('SonarQube Analysis') {
    def scannerHome = tool 'SonarScanner_for_MSBuild'
    withSonarQubeEnv() {
      bat "${scannerHome}\\SonarScanner.MSBuild.exe begin /k:\"scandotnetcorewithjenkins\""
      bat "dotnet build"
      bat "${scannerHome}\\SonarScanner.MSBuild.exe end"
    }
  }
}
