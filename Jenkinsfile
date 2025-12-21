pipeline {
    agent any

    environment {
        SPRING_PROFILE = "test"
    }

    stages {
        stage('Checkout') {
            steps {
                git(
                    url: 'https://github.com/Tasnim70/MonProjetMaven.git',
                    branch: 'main',
                    credentialsId: 'github-creds'
                )
            }
        }

        stage('Build') {
            steps {
                sh "mvn clean package -Dspring.profiles.active=${SPRING_PROFILE}"
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sq1') {
                    sh "mvn sonar:sonar -Dspring.profiles.active=${SPRING_PROFILE}"
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline terminé avec succès !'
        }
        failure {
            echo 'Échec du pipeline !'
        }
        always {
            cleanWs() // <-- pas besoin de node ici dans declarative pipeline
        }
    }
}
