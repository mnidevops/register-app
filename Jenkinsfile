pipeline {
	tools {
		maven 'maven3.9.6'
		jdk 'Java17'
	}
	agent {
		label 'Jenkins-Agent'
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
		stage ('Build application package') {
			steps {
				sh "mvn clean package"
			}
		}
		stage ('Test application') {
			steps {
				sh "mvn test"
			}
		}
		stage ('Check Code Quality') {
			steps {
				withSonarQubeEnv(credentialsId: 'sonarqube-creds', installationName: 'sonarqube-server') {
					sh "mvn sonar:sonar"
				}
			}
		}
		stage ('QualityGate check') {
			steps {
				waitForQualityGate abortPipeline: false, credentialsId: 'sonarqube-creds'
			}
		}
		
	}
}