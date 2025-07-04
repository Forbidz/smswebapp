node {
  stage('SCM') {
    checkout scm
  }
  stage('ScannerQube Analysis')
    def scannerHome = tool 'SonarScanner for .MSBuild'
    withSonarQubeEnv() {
      bat "dotnet ${scannerHome}\\SonarScanner.MSBuild.dll begin /k:\"scadotnetcorewithjenkins""
      bat "dotnet build"
      bat "dotnet ${scannerHome}\\SonarScanner.MSBuild.dll end"
    }
  }
}
