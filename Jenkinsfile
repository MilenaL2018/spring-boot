pipeline {
    environment {
        registry = "milelucero98/test-spring-boot"
        registryCredential = 'docker-hub-credentials'
        dockerImage = ''
    }
    agent any
    stages {
        stage('Clone Git') {
            steps {
                git 'https://github.com/MilenaL2018/spring-boot-tp7.git'
            }
        }
        stage('Build image') {
            steps {
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
        stage('Deploy image') {
            steps {
                script {
                    docker.withRegistry( '', registryCredential ) {
                        dockerImage.push()
                    }
                }
            }
        }
        
          stage('Prepare tests') {
            steps{
                sh script:'''
                  #!/bin/bash
                  mvn -Dmaven.test.failure.ignore=true clean package
                  cd ./it
                  npm install
                '''
            }
        }
        
        stage('Tests') {
            steps {
                sh script:'''
                  #!/bin/bash
                  java -jar target/spring-boot-sample-actuator-2.0.2.jar &
                  sleep 7
                  cd ./it
                  npx codeceptjs run --steps --reporter mocha-multi
                '''
            }
            
            post {
                success {
                    junit '**/output/*.xml'
                }
            }
        }
        
        stage('Static code analisis') {
            steps {
                sh 'mvn sonar:sonar -Dsonar.projectKey=MilenaL2018_spring-boot-tp7 -Dsonar.organization=milenal2018 -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=ec90eff6cf079c3c4e688812bcd92291ecf52eb2 -Dmaven.test.failure.ignore=true -Dsonar.scanner.force-deprecated-java-version=true'
            }
        }
        
        stage('Cleaning up') {
            steps { 
                sh "docker rmi $registry:$BUILD_NUMBER" 
            }
        } 
    }
}
