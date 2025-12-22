pipeline {
    agent any
    stages {
        stage('Check Java') {
            steps {
                sh 'java -version'
            }
        }
    
    environment {
        SONAR_TOKEN = credentials('4SAE-project-token')
    }
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/Tasnim70/MonProjetMaven.git', branch: 'main'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                sh "mvn sonar:sonar -Dsonar.projectKey=4SAE-project -Dsonar.host.url=http://192.168.33.10:9000 -Dsonar.login=$SONAR_TOKEN"
            }
        }
    }
}
