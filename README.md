# Node One Page

Steps to configure Jenkins SonarQube integration:\
(Official documentation  [https://docs.sonarqube.org/latest/analysis/scan/sonarscanner-for-jenkins/](https://docs.sonarqube.org/latest/analysis/scan/sonarscanner-for-jenkins/):
1. Install SonarScanner Jenkins plugin (and SonarQube gates if needed):
![Jenkins plugins](img/plugins.png)
2. Generate Sonar Server authorization token (under Sonar Admininstration panel) and add it to Jenkins credentials\
3. Configure Sonar Server in Jenkins Global Configuration:
![Sonar Server](img/sonarServer.png)
4. Configure Sonar Scanner in Global Tool Configuration:
![Sonnar Scanner](img/sonarScannerTool.png)

