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
					 def RepoName = mavenPom.version.endsWith("SNAPSHOT") ? "${mavenPom.name}-snapshot" : "${mavenPom.name}-release"
				  sshPublisher(
				   continueOnError: false, failOnError: true,
				   publishers: [
					sshPublisherDesc(
					 configName: "localhost",
					 verbose: true,
					 transfers: [
					  sshTransfer(
					   sourceFiles: "target/myapp-${mavenPom.version}.war",
					   removePrefix: "target/",
					   remoteDirectory: "/mywordpress/${RepoName}",
					   execCommand: ""
					  )
					 ])
				   ])
				 }
		 }
		}
    }
}
