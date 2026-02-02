pipeline {
	agent any

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
	}
}