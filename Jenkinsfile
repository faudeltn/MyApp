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
		    steps{
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
					   sourceFiles: "target/*.war",
					   removePrefix: "target/",
					   remoteDirectory: "/mywordpress",
					   execCommand: ""
					  )
					 ])
				   ])
				 }
		 }
		}
    }
}
