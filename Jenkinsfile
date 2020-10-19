pipeline {
    environment {
        registry = "milelucero98/test-spring-boot-tp7"
        registryCredential = 'dockerhub/milelucero98'
        dockerImage = 'milelucero98/test-spring-boot-tp7'
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
        stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
            docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
            }
        }
        stage('Cleaning up') {
                steps { 
                sh "docker rmi $registry:$BUILD_NUMBER" 
                    }
        } 
    }
}
