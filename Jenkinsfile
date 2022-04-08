pipeline {
    agent any

    stages {
        stage('Git Checkout') {
            steps {
                git 'https://github.com/vickom00/Sample_project.git'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonar') {
                    sh 'mvn -f SampleWebApp/pom.xml clean package sonar:sonar'
                }
            }
        }
        stage('Quality Gate') {
            steps {
                waitForQualityGate abortPipeline: true, credentialsId: 'sonar-cred'
            }
        }
        stage('Test with Maven') {
            steps {
                sh 'cd SampleWebApp && mvn test'
            }
        }
        stage('Build with Maven') {
            steps {
                sh 'cd SampleWebApp && mvn clean package -Dbuild.number=${BUILD_NUMBER}'
            }
        }
        stage('Deploy artificat to Jfrog') {
            steps {
                rtDownload (
                    serverId: 'first-jfrog',
                    spec: '''{
                          "files": [
                            {
                              "pattern": "/var/lib/jenkins/workspace/test-pipeline/SampleWebApp/target/*.war",
                              "target": "my-first-repo/"
                            }
                          ]
                    }''',
                  )
            }
        }
        stage('Deploy to Tomcat') {
            steps {
                deploy adapters: [tomcat9(credentialsId: 'devop1', 
                path: '', 
                url: 'http://54.82.111.65:8080/default')], 
                contextPath: 'default', 
                war: '**/*.war'
            }
        }    
    }
}
