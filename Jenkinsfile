pipeline {
    agent any

    tools {
        maven 'M2_HOME'   // Nom du Maven configuré dans Jenkins
        jdk 'JAVA_HOME'    // Nom du JDK configuré dans Jenkins
    }

    environment {
        SONAR_TOKEN = credentials('jenkins-sonar')  // Ton token SonarQube
    }

    stages {

        stage('Checkout Git') {
            steps {
                // Repo public, pas besoin de credentials
                git branch: 'main',
                    url: 'https://github.com/Tasnim70/MonProjetMaven.git'
            }
        }

        stage('Clean') {
            steps {
                sh 'mvn clean'
            }
        }

        stage('Compile') {
            steps {
                sh 'mvn compile'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sqsd') {  // Nom du serveur SonarQube dans Jenkins
                    sh "mvn sonar:sonar -Dsonar.login=${SONAR_TOKEN}"
                }
            }
        }

        stage('Package (JAR)') {
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
        always {
            node {
                cleanWs()  // Nettoie le workspace à chaque build
            }
        }
        success {
            echo 'Pipeline CI réussi ! 🎉'
        }
        failure {
            echo 'Échec du pipeline ❌'
        }
    }
}
