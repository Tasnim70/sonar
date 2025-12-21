node {
    stage('Checkout') {
        checkout scm
    }

    stage('Build & Sonar') {
        withSonarQubeEnv('sq1') {
            sh "mvn clean package sonar:sonar -Dspring.profiles.active=test"
        }
    }

    stage('Cleanup') {
        cleanWs()
    }
}
