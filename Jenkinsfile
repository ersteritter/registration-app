pipeline {
    agent { label 'jenkins-agent' }
    environment {
	APP_NAME = "register-app-pipeline"
        RELEASE = "1.0.0"
        DOCKER_USER = "ersteritter"
        DOCKER_PASS = 'dockerhub'
        IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
        IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
	## JENKINS_API_TOKEN = credentials("JENKINS_API_TOKEN")
    }
    tools {
      jdk 'Java17'
      maven 'Maven3'
    }
    stages {
        stage('Clean-Workspace') {
                steps {
                cleanWs()
            }
        }
        stage('Ckeckout from SCM') {
                steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/ersteritter/registration-app.git'
            }
        }
        stage('Buil-Application') {
                steps {
                sh "mvn clean package"
            }
        }
        stage('Test-Application') {
                steps {
                sh "mvn test"
            }
        }
          stage("Build & Push Docker Image") {
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
