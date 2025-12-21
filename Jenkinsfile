pipeline {
    agent none

    stages {
        stage('build & SonarQube Scanner') {
            agent any
            steps {
                withSonarQubeEnv('cube') {
                    sh 'mvn clean package sonar:sonar'
                }
            }
        }
    }
}
