pipeline{
    agent{
        label "jenkins-agent"
    }
    tools {
        jdk 'Java17'
        maven 'Maven3'
    }
    environment {
        APP_NAME = "production-e2e-pipeline"
        RELEASE = "1.0.0"
        DOCKER_USER = "devintripp"
        DOCKER_PASS = "dockerhub"
        IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
        IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
    }
    stages{
        stage("Cleanup Workspace"){
            steps{
                echo "========executing Cleanup Workspace========"
                cleanWs()

            }
        }
        stage("Checkout from SCM"){
            steps{
                echo "========Checkout from SCM========"
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/devinshawntripp/production-e2e-pipeline'

            }
        }
        stage("Build Application"){
            steps{
                echo "========Build Application========"
                sh "mvn clean package"

            }
        }
        stage("Test Application"){
            steps{
                echo "========Test Application========"
                sh "mvn test"

            }
        }
        stage("Sonarqube anaylisis"){
            steps{
                echo "========Sonarqube anaylisis========"
                script {
                    withSonarQubeEnv(credentialsId: 'jenkins-sonarqube-token') {
                        sh "mvn sonar:sonar"
                    }
                }
                

            }
        }
        stage("Quality Gate"){
            steps{
                echo "========Sonarqube anaylisis========"
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'jenkins-sonarqube-token'
                }
                

            }
        }
        stage("Build and Push Docker Image"){
            steps{
                echo "========Build and Push Docker Image========"
                script {
                    docker.withRegistry('', DOCKER_PASS){
                        docker_image = docker.build "${IMAGE_NAME}"
                    }

                    docker.withRegistry('', DOCKER_PASS){
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push('latest')
                    }

                }
                

            }
        }
    }

}