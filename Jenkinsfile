pipeline {
    agent any

    stages {
        stage('Git Checkout') {
            steps {
                git 'https://github.com/vickom00/Sample_project.git'
            }
        }
        stage('Test with Maven') {
            steps {
                sh 'cd SampleWebApp && mvn test'
            }
        }
        stage('Build with Maven') {
            steps {
                sh 'cd SampleWebApp && mvn package'
            }
        }
        stage('Deploy to Tomcat') {
            steps {
                deploy adapters: [tomcat9(credentialsId: 'devop1', 
                path: '', 
                url: 'http://54.159.122.196:8080/default')], 
                contextPath: 'default', 
                war: '**/*.war'
            }
        }        
    }
}