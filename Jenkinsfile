pipeline {
	tools {
		maven 'maven3.9.6'
		jdk 'Java17'
	}
	stages {
		stage ('Clean Workspace') {
			steps {
				cleanWs()
			}
		}

		stage ('Checkout code') {
			steps {
				git branch: 'develop', credentialsId: 'jenkins-creds', url: 'https://github.com/mnidevops/register-app.git'
			}
		}
		
	}
}