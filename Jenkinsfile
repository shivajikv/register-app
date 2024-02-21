pipelines {
    agent{
        label 'jenkins-agent'
    }
    tools {
        jdk 'java17'
        maven 'Maven3'
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
    }
}
