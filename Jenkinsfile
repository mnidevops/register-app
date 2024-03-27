pipeline {
	tools {
		maven 'maven3.9.6'
		jdk 'Java17'
	}
	agent {
		label 'Jenkins-Agent'
	}
	environment {
		APP_NAME = "register-app"
		RELEASE = "1.0.0"
		DOCKER_USER = "mnidevops"
		DOCKER_PASS = 'docker-hub'
		IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
		IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
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
		stage ('Build and Push Docker Image') {
			steps {
				script {
					docker.withRegistry('',DOCKER_PASS) {
						docker_image = docker.build "${IMAGE_NAME}"
					}

					docker.withRegistry('',DOCKER_PASS) {
						docker_image.push("${IMAGE_TAG}")
						docker_image.push('latest')
					}
				}
			}
		}
		
	}
}