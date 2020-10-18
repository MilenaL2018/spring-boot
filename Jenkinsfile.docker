pipeline {
    environment {
        registry = "milelucero98/test-spring-boot-tp7"
        registryCredential = 'dockerhub/milelucero98'
        dockerImage = 'test-spring-boot-tp7'
    }
    agent any
    stages {
        stage('Cloning our Git') {
            steps {
                git 'https://github.com/MilenaL2018/spring-boot-tp7.git'
            }
        }
        stage('Building our image') {
            steps {
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
        stage('Deploy our image') {
            steps {
                script {
                    docker.withRegistry( '', registryCredential ) {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Cleaning up') {
            steps { 
                sh "docker rmi $registry:$BUILD_NUMBER" 
            }
        } 
    }
       
} 
