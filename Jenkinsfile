pipeline {
    agent any
    tools {
        maven 'maven3'
    }
    options {
        buildDiscarder logRotator(daysToKeepStr: '5', numToKeepStr: '7')
    }
    stages{
        stage('Build'){
            steps{
                 sh script: 'mvn clean package'
                 archiveArtifacts artifacts: 'target/*.war', onlyIfSuccessful: true
            }
        }
		stage('SSH transfer') {
		 script {
			 def mavenPom = readMavenPom file: 'pom.xml'
		  sshPublisher(
		   continueOnError: false, failOnError: true,
		   publishers: [
			sshPublisherDesc(
			 configName: "localhost",
			 verbose: true,
			 transfers: [
			  sshTransfer(
			   sourceFiles: "target/myapp-${mavenPom.version}.war",
			   #removePrefix: "${path_to_file}",
			   remoteDirectory: "/",
			   execCommand: "run commands after copy?"
			  )
			 ])
		   ])
		 }
		}
    }
}
