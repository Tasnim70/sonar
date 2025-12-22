pipeline {
    agent none

    tools {
        maven 'M2_HOME'   // Nom du Maven configuré dans Jenkins
        jdk 'JAVA_HOME'    // Nom du JDK configuré dans Jenkins
    }

    stages {
        stage('Build & SonarQube Scanner') {
            agent any
            steps {
                withSonarQubeEnv('sq1') { // Nom de ton serveur SonarQube
                    sh 'mvn clean package sonar:sonar -Dspring.profiles.active=test'
                }
            }
        }
    }

    post {
        always {
            echo 'Nettoyage du workspace...'
            cleanWs()
        }
        success {
            echo 'Pipeline terminé avec succès !'
        }
        failure {
            echo 'Échec du pipeline !'
        }
    }
}
