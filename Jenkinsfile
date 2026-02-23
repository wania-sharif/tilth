pipeline {
	agent any
	
	environment {
		CONTAINER_NAME = "tilth-website-${env.BRANCH_NAME}"
		PORT = "${env.BRANCH_NAME == 'development' ? '8001' : '8000'}"
	}
	stages {
		stage('fetch') {
			steps {
				sh """
				git checkout ${env.BRANCH_NAME}
				mkdir samplesite
				mv *.html samplesite/"""
			}
		}
		stage('build') {
			steps {
				sh "docker build -t ${CONTAINER_NAME} ."
			}
		}
		stage('deploy') {
			steps {
				sh """
				docker stop ${CONTAINER_NAME} || true && docker rm ${CONTAINER_NAME} || true
				docker run -dit --name ${CONTAINER_NAME} -p ${env.BRANCH_NAME == 'main' ? '8000' : '8001'}:80 ${CONTAINER_NAME}"""
			}
		}
		stage('cleanup') {
			steps {
				cleanWs()
			}
		}
	}
}
