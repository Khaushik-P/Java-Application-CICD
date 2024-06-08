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
       stage("SonarQube Analysis"){
           steps {
	           script {
		        withSonarQubeEnv(credentialsId: 'sonar-scanner') { 
                        sh "mvn sonar:sonar"
		        }
	           }	
           }
       }
       stage("Quality Gate"){
           steps {
               script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-scanner'
                }	
            }

        }
    }
}