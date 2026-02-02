pipeline {
	agent any


	environment {
		APP_NAME = "demo-app"
		RELESE = "1.0.0"
		DOCKER_USER = "younes015"
		DOCKER_PASS = "docker-cred"
		IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
		IMAGE_TAG = "${RELESE}" + "-" + "${BUILD_NUMBER}"
	}
	
	stages {
		stage('clean workspace') {

			steps {
				cleanWs()
			}
		}
		stage('checkout') {
			steps {
				git branch : 'main', credentialsId : 'github_id', url : 'https://github.com/younes200444/cicdTest.git'
			}
		}
		stage('build') {
			steps {
				sh 'mvn clean package'
			}
		}
		stage('test') {
			steps {
				sh 'mvn test'
			}
		}
		stage('sonarqube check') {
			steps {
				script {
					withSonarQubeEnv(credentialsId: 'sonarqube') {
						sh 'mvn sonar:sonar'
					}
				}
			}
		}
		stage('qualit Gate') {
			steps {
				script {
					waitForQualityGate abortPipline: false, credentialsId: 'sonarqube'
				}
			}

		}
		stage('build docker image') {
			steps {
				script {
					docker.withRegistry('', DOCKER_PASS) {
						docker_image = docker.build "${IMAGE_NAME}"
					}

					echo "🛡️ Démarrage du scan de sécurité Trivy..."
					sh "curl -sfL https://raw.githubusercontent.com/aquasecurity/trivy/main/contrib/install.sh | sh -s -- -b ."
					sh "./trivy image --exit-code 1 --severity CRITICAL --no-progress --ignorefile .trivyignore ${IMAGE_NAME}"
					echo "✅ Scan terminé : Aucune faille critique détectée."

					docker.withRegistry('', DOCKER_PASS) {
						docker_image.push("${IMAGE_TAG}")
						docker_image.push("latest")
					}
				}
			}
		}

	}
}