pipeline {
  agent {
    label 'master'
  }
  
  environment {
      sonarServer = "sonar-cloud"
      sonarScanner = "sonar-scanner-linux"
      sonarMainBranch = "master"
  }

 tools {
    jdk 'openjdk-11'
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
      when {
        expression {
          env.BRANCH_NAME.matches ("master") || env.BRANCH_NAME.matches ("develop") || env.BRANCH_NAME.matches ("sonar-cloud") || env.BRANCH_NAME.contains ("PR-") || env.BRANCH_NAME.contains ("release")
        }
      }
      steps {
        script {
          def sonarQubeScanner = tool "${env.sonarScanner}"
          withSonarQubeEnv("${env.sonarServer}") {
            if (env.BRANCH_NAME == env.sonarMainBranch) {
              sh "${sonarQubeScanner}/bin/sonar-scanner -Dproject.settings./sonar-project.properties"
            } else {
              sh "${sonarQubeScanner}/bin/sonar-scanner -Dsonar.branch.name=${env.BRANCH_NAME} -Dproject.settings./sonar-project.properties"
            }
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
