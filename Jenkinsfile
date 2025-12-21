pipeline {
    agent any

    tools {
        maven 'M2_HOME'       // Nom du Maven configuré dans Jenkins
        jdk 'JAVA_HOME'       // Nom du JDK configuré dans Jenkins
    }

    environment {
        SONAR_TOKEN = credentials('sonar')  // Token SonarQube
    }

    stages {
        stage('Checkout Git') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Tasnim70/MonProjetMaven.git'
                    // Si repo privé, ajoute : credentialsId: 'github-creds'
            }
        }

        stage('Clean & Build') {
            steps {
                sh 'mvn clean verify'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sq1') {   // Nom du serveur SonarQube dans Jenkins
                    sh "mvn sonar:sonar -Dsonar.login=${SONAR_TOKEN}"
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Package JAR') {
            steps {
                sh 'mvn package -DskipTests'
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
            }
        }
    }

    post {
        success {
            echo 'Pipeline CI réussi !'
        }
        failure {
            echo 'Échec du pipeline'
        }
    }
}
