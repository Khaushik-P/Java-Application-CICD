pipeline{
    agent {label 'Jenkins-Agent'}
    tools{
        jdk 'Java17'
        maven 'Maven3'
    }
    // environment{

    // }
    stages{
        stage("Clean Workspace"){
            steps{
              cleanWs()
            }
        }
        stage("Git Checkout"){
            steps{
                git branch: 'main',credentialsId: 'github',url: 'https://github.com/Khaushik-P/Java-Application-CICD.git'
            }
        }
        stage("Build Application"){
            steps{
                sh 'mvn clean package'
            }
        }
        stage("Test Application"){
            steps{
                sh 'mvn test'
            }
        }
        stage('SonarQube Analysis') {
          def mvn = tool 'Maven3';
          withSonarQubeEnv() {
            sh "${mvn}/bin/mvn clean verify sonar:sonar -Dsonar.projectKey=JAVA_CICD -Dsonar.projectName='JAVA_CICD'"
          }
        }
    }
}