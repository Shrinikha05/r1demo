pipeline {
	agent any
	
	
	environment {
		DOCKERHUB_CRED=credentials('dockerhub')
		IMAGE_NAME="shrinikha05/react_app"
	}
	
	stages {
		stage('checkout') {
			steps {
			  git credentialsId:'github_token', url:'https://github.com/Shrinikha05/r1demo.git', branch:'main'
				
			}
		}
		

		stage('Install Dependencies and Build React App') {
			steps {
				retry(3) {
					sh 'npm install'
					sh 'npm run build'
				}
			}
		}




		stage('Build Docker Image') {
			steps {
				retry(3) {
					script {
						dockerImage=docker.build("${IMAGE_NAME}:latest")
					}
				}
			}
		}
		
		stage('Push to DockerHub') {
			steps {
				retry(3) {
					script {
						docker.withRegistry('https://index.docker.io/v1/','dockerhub') {
							dockerImage.push()
						}
					}
				}
			}
		}
	}
	
	post {
		success {
			echo "Pipeline Successful"
		}
		failure {
			echo "Pipeline Failed"
		}
		always {
			echo "Cleaning Up WorkSpace"
			deleteDir()
		}
	}
}
