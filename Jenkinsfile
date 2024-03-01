pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                 bat "C:/apache-maven-3.9.5/bin/mvn clean package"
            }
        }
        stage('Test') {
            steps {
                 bat "C:/apache-maven-3.9.5/bin/mvn test"
            }
        }
         stage('Deploy') {
            steps {
                 bat 'C:/Docker/resources/bin/docker-compose up -d --build'
            }
        }
         stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub_credentials', passwordVariable: 'DOCKER_HUB_PASSWORD', usernameVariable: 'DOCKER_HUB_USERNAME')]) {
                    bat "C:/Docker/resources/bin/docker login -u $DOCKER_HUB_USERNAME -p $DOCKER_HUB_PASSWORD"
                    bat "C:/Docker/resources/bin/docker tag evalspringse:latest lamineoziljr/evalspringse:v$BUILD_NUMBER"
                    bat "C:/Docker/resources/bin/docker push lamineoziljr/evalspringse:v$BUILD_NUMBER"
               }
            }
       }
    }
}
