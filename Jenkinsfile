pipeline {
	agent {
		docker {
			image 'younes015/maven-jenkins-agent:v1'
			args '--user root -v /var/run/docker.sock:/var/run/docker.sock'
		}
	}

	stages {
		stage('clean workspace') {

			steps {
				cleanWs()
			}
		}
		stage('checkout') {
			steps {
				git branch : 'main', credentialsId : 'github_id', url : 'TODO'
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
	}
}