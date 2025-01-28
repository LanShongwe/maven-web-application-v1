node{

def mavenHome = tool name: 'maven3.9.8'

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([pollSCM('* * * * *')])])


stage('CheckOutCode'){
git branch: 'development', credentialsId: 'fac38c0c-86fd-48e4-8733-eefd41f2d3d7', url: 'https://github.com/MithunTechnologiesDevOps/maven-web-application.git'
}

stage('Build'){
sh "${mavenHome}/bin/mvn clean package"
}

/*
stage('ExecuteSonarQubeReport'){
sh "${mavenHome}/bin/mvn clean sonar:sonar"
}

stage('UploadArtifactsIntoNexus'){
sh "${mavenHome}/bin/mvn clean deploy"
}

stage('DeployAppintoTomcatServer'){
sshagent(['49f3f06f-0166-44c9-8e4e-7309df0b03a8']) {
  sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.24.100:/opt/apache-tomcat-9.0.98/webapps/"  
}
}

*/
  
}
