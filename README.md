# Node One Page

Steps to configure Jenkins SonarQube integration:
(Official documentation  [https://docs.sonarqube.org/latest/analysis/scan/sonarscanner-for-jenkins/](https://docs.sonarqube.org/latest/analysis/scan/sonarscanner-for-jenkins/):
1. Install SonarScanner Jenkins plugin (and SonarQube gates if needed):
![Jenkins plugins](img/plugins.png) <br/>
2. Generate Sonar Server authorization token (under Sonar Admininstration panel) and add it to Jenkins credentials\
3. Configure Sonar Server in Jenkins Global Configuration:
![Sonar Server](img/sonarServer.png) <br/>
4. Configure Sonar Scanner in Global Tool Configuration:
![Sonnar Scanner](img/sonarScannerTool.png) <br/>

### Single branch only (master) (Sonar Community edition 8.x.x and up)
Community edition allows only one branch.
[SingleBranchJenkinsfile](SingleBranchJenkinsfile) is the pipeline which scans master branch only.
### Multibranch Jenkins job
Developer and Enterprise editions give you posibility to scan multiple branches.

This Jenkinspipeline section allows specific branch patterns to scan:
```
when {
  expression {
    env.BRANCH_NAME.matches ("master") || env.BRANCH_NAME.matches ("develop") || env.BRANCH_NAME.contains ("PR-") || env.BRANCH_NAME.contains ("release")
  }
}
```
Also, you may want to specify *Administration => Branches & Pull Requests => Long-lived branches pattern:* which will keep scans from specific branches (the rest will be clean after some period of time).

**Note**: main branch should be scanned first! (sonarMainBranch variable)

### Sonar Cloud
SonarCloud supports multiple branches and the configuration is very simular to on-premise one.
The only one difference is that it requires an extra key in sonar-project.properties:
```
sonar.organization=<your organization name>
```
The pipeline itself is configured in [sonar-cloud](https://github.com/vi2co/node-one-page/tree/sonar-cloud) branch.

### SonarGates
SonarGates should be configured in SonarQube to fail the build in case if the code doesn't match requirements.
