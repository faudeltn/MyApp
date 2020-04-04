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
		  sshPublisher(
		   continueOnError: false, failOnError: true,
		   publishers: [
			sshPublisherDesc(
			 configName: "${env.SSH_CONFIG_NAME}",
			 verbose: true,
			 transfers: [
			  sshTransfer(
			   sourceFiles: "${path_to_file}/${file_name}, ${path_to_file}/${file_name}",
			   removePrefix: "${path_to_file}",
			   remoteDirectory: "${remote_dir_path}",
			   execCommand: "run commands after copy?"
			  )
			 ])
		   ])
		 }
		}
    }
}
