pipeline {
    environment {
        registry = 'milelucero98/test-spring-boot-tp7'
        dockerImage = ''
    }
    agent any
    stages {
        stage('Cloning our Git') {
            steps {
                git 'https://github.com/MilenaL2018/spring-boot-tp7.git'
            }
        }
        stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */
            steps{
                dockerImage = docker.build("milelucero98/test-spring-boot-tp7") + ":$BUILD_NUMBER"
            }
        }
        stage('Deploy our image') {
             steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
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
