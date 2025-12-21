pipeline {
    agent none

    stages {
        stage('build & SonarQube Scanner') {
            agent any
            steps {
                withSonarQubeEnv('sq1') {
                    sh 'mvn clean package sonar:sonar'
                }
            }
        }
    }
}
