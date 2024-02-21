pipeline {
    agent{
        label 'jenkins-agent'
    }
    tools {
        jdk 'java17'
        maven 'Maven3'
    }
    environment {
	    APP_NAME = "register-app-pipeline"
            RELEASE = "1.0.0"
            DOCKER_USER = "shivajikv"
            DOCKER_PASS = 'kvshivaji'
            IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
            IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
	    JENKINS_API_TOKEN = credentials("JENKINS_API_TOKEN")
    }	
    stages{
        stage ("Cleanup-workspace"){
            steps{
                cleanWs()
            }
        }
        stage("checkout from SCM"){
            steps{
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/shivajikv/register-app.git'
            }
        }
        stage("Build-applicatin"){
            steps{
                sh 'mvn clean package'
            }
        }
        stage("TEST"){
            steps {
                sh 'mvn test'
            }
        }
        stage("sonar-analysis"){
            steps {
                script {
		     withSonarQubeEnv(credentialsId: 'jenkis-sonar-token') { 
                     sh "mvn sonar:sonar"
		     }
               }
          }
    }
      stage("Quality Gate"){
           steps {
               script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'jenkins-sonarqube-token'
                }	
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
	
