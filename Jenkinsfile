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
    }
}
