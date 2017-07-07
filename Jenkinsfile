node {
  // given molgenis-version from Jenkins
  def molgenisVersion = molgenisVersion
  def gitRemoteUser = 'jenkins'
  def gitRemoteUrl = 'github.com/sidohaakma/molgenis-docker'
  def artifactId = "molgenis-docker"
  stage('Preparation') {
    // Clean workspace
    step([$class: 'WsCleanup', cleanWhenFailure: false])
    // Get code from github.com
    checkout changelog: false, poll: false, scm: [$class: 'GitSCM', branches: [[name: 'master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'jenkins-git', url: 'http://' + gitRemoteUser + '@' + gitRemoteUrl + '.git']]]
    // Get the Maven tool.
    mvnHome = tool 'MAVEN'
    // set display name
    currentBuild.displayName = pom.version
  }
  stage('Test') {
    echo "Test artifact : ${artifactId}"
  }
  stage('Build') {
    echo "Build artifact : ${artifactId}"
    docker build molgenis/${molgenisVersion} -t repository.haakma.org/molgenis:${molgenisVersion}

  }
  stage('Deploy')
    docker push repository.haakma.org/molgenis:${molgenisVersion}
  stage('Acceptance')
    currentBuild.result = 'SUCCESS';
  stage('Release') {
    echo "Deploy artifact : ${artifactId}";
    // deploy it in the staging repo
    def userInput = input(
     id: 'userInput', message: 'Release to production?', parameters: [
     [$class: 'TextParameterDefinition', defaultValue: 'uat', description: 'Environment', name: 'env'],
     [$class: 'TextParameterDefinition', defaultValue: 'uat1', description: 'Target', name: 'target']
    ])
    currentBuild.result = 'SUCCESS';
  }
}