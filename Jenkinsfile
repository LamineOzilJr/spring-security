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
                 def programFilesShortName = bat(script: 'dir /X C:\\ | find "Program Files"', returnStatus: true).trim()
                 bat "C:/${programFilesShortName}/Docker/Docker/resources/bin/docker-compose/docker-compose up -d --build"
            }
        }
    }
}
