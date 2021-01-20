pipeline {
agent {
  label 'master'
}
  stages {
    stage('Build') {
      steps {
        sh """
          npm install
        """
      }
    }
  
    stage('Sonar') {
      steps {
        script {
          def sonarQubeScanner = tool "sonar-scanner-linux-from-global-tools"
          withSonarQubeEnv('sonar-server-name-from-jenkins-global-config') {
            sh "${sonarQubeScanner}/bin/sonar-scanner -Dproject.settings./sonar-project.properties"
            // More parameters https://docs.sonarqube.org/latest/analysis/analysis-parameters/
          }

          // In case of troubles with Sonar gates: 
          // https://community.sonarsource.com/t/need-a-sleep-between-withsonarqubeenv-and-waitforqualitygate-or-it-spins-in-in-progress/2265
          // sleep ("10") // wait 10 minutes
          // timeout(time: 10, unit: 'MINUTES') {
          //   waitForQualityGate abourtPipeline: true
          // }
        }
      }
    }
  }
}
