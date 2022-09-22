node{
 
  //echo "GitHub BranhName ${env.BRANCH_NAME}"
  //echo "Jenkins Job Number ${env.BUILD_NUMBER}"
  echo "Jenkins Node Name ${env.NODE_NAME}"
  
  echo "Jenkins Home ${env.JENKINS_HOME}"
  echo "Jenkins URL ${env.JENKINS_URL}"
  echo "JOB Name ${env.JOB_NAME}"
  
  properties([
    buildDiscarder(logRotator(numToKeepStr: '3')),
    pipelineTriggers([
        pollSCM('* * * * *')
    ])
  ])
 
    def mavenhome= tool name: 'maven.3.8.6', type: 'maven' 
    buildName 'dev-env_${BUILD_NUMBER}'
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '1', numToKeepStr: '3')), [$class: 'RebuildSettings', autoRebuild: false, rebuildDisabled: false], [$class: 'JobLocalConfiguration', changeReasonComment: '']])
    stage('getting code from the github'){
        git 'https://github.com/Manohar4888/maven-web-application.git'
    }

    stage('build using maven'){
        sh "${mavenhome}/bin/mvn clean package"
    }
    
    stage('sonarqubereport'){
        sh "${mavenhome}/bin/mvn sonar:sonar"
    }
    stage('deploy to tomcat'){
    sshagent(['ssh-agent-2']) {
   sh "scp -o StrictHostkeyChecking=no target/maven-web-application.war  ec2-user@13.235.104.91:/opt/apache-tomcat-9.0.65/webapps/"
    }
}
}
