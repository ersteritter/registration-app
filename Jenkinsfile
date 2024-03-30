pipeline {
    agent { label 'jenkins-agent' }
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
    }
}
