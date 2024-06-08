pipeline{
    agent {label 'Jenkins-Agent'}
    tools{
        jdk 'Java17'
        maven 'Maven3'
    }
    environment {
	    APP_NAME = "java-cicd"
        RELEASE = "1.0.0"
        DOCKER_USER = "khaushik"
        DOCKER_PASS = 'docker'
        IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
        IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
	    // JENKINS_API_TOKEN = credentials("")
    }
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
        stage("Build and push to docker hub"){
            steps{
                script{
                    docker.withRegistry('',DOCKER_PASS){
                        docker_image = docker.build "${IMAGE_NAME}"
                    }
                    docker.withRegistry('',DOCKER_PASS){
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push('latest')
                    }
                }
            }
        }

        stage("Trivy Image scan"){
            steps{
                sh 'trivy image khaushik/java-cicd:latest > trivyimage.txt'
            }
        }
       stage ('Cleanup Artifact') {
           steps {
               script {
                    sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
                    sh "docker rmi ${IMAGE_NAME}:latest"
               }
          }
       }
    }
}