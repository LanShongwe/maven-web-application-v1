try {
	node {
		properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([pollSCM('* * * * *')])])
			
		def mavenHome = tool name: "maven3.8.6"
					
		echo "The Job name is: ${env.JOB_NAME}"
		echo "The Node name is: ${env.NODE_NAME}"
		echo "The Build Number is: ${env.BUILD_NUMBER}"
		echo "The Jenkins Home directory is: ${JENKINS_HOME}"

		sendSlackNotifications("STARTED")
					
		stage('CheckoutCode'){
			git branch: 'development', 
			credentialsId: '28ecb148-5261-4e78-b0e1-c4d8fdaca82f', 
			url: 'https://github.com/LanShongwe/maven-web-application-v1.git'
		}
				
		stage ('Build'){
			sh "${mavenHome}/bin/mvn clean package"
		}
				
		stage ('ExecuteSonarQubeReport'){
			sh "${mavenHome}/bin/mvn sonar:sonar"
		}
				
		stage ('UploadArtifactsIntoNexus') {
			sh "${mavenHome}/bin/mvn deploy"
		}
					
		stage ('DeployAppIntoTomcatServer') {
			sshagent(['7f844fa7-69c4-4894-a788-013e712840a3']) {
				sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.29.29:/opt/apache-tomcat-9.0.102/webapps/"
			}	
		}
	}
} catch(e) {
	currentBuild.result = "FAILURE"
		
} finally {
	sendSlackNotifications(currentBuild.result)
}
	
// Slack Send Notifications
def sendSlackNotifications(String buildStatus = 'STARTED') {
	// build status of null means successful
	buildStatus =  buildStatus ?: 'SUCCESS'

	// Default values
	def colorName = 'RED'
	def colorCode = '#FF0000'
	def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
	def summary = "${subject} (${env.BUILD_URL})"

	// Override default values based on build status
	if (buildStatus == 'STARTED') {
		colorName = 'ORANGE'
		colorCode = '#FFA500'
	} else if (buildStatus == 'SUCCESS') {
		colorName = 'GREEN'
		colorCode = '#00FF00'
	} else {
		colorName = 'RED'
		colorCode = '#FF0000'
	}
	// Send notifications
	slackSend (color: colorCode, message: summary)
}
