node {
  stage('Build + SonarQube analysis') {
    def sqScannerMsBuildHome = tool 'Scanner for .Net Framework'
    withSonarQubeEnv(<sonarqubeInstallation>') {// If you have configured more than one global server connection, you can specify its name as configured in Jenkins
      bat "${sqScannerMsBuildHome}\\SonarScanner.MSBuild.exe begin /k:myKey"
      bat 'MSBuild.exe /t:Rebuild'
      bat "${sqScannerMsBuildHome}\\SonarScanner.MSBuild.exe end"
    }
  }
}
