pipeline{
    agent {label 'Jenkins-Agent'}
    tools{
        jdk 'Java17'
        maven 'Maven3'
    }
    environment{

    }
    stages{
        stage("Clean Workspace"){
            steps{
              cleanWs()
            }
        }
        stage("Git Checkout"){
            script{
                git branch: 'main',credentialsId: 'github',url: ''
            }
        }
    }
}