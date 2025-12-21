pipeline {
    agent any

    tools {
        maven 'M2_HOME'   // doit exister dans Global Tool Configuration
        jdk 'JAVA_HOME'    // doit exister dans Global Tool Configuration
    }

    environment {
        SONAR_TOKEN = credentials('jenkins-sonar')
    }

    stages {
        stage('Checkout Git') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Tasnim70/MonProjetMaven.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sq1') {
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }

    post {
        always {
            deleteDir()
        }
        success {
            echo 'Pipeline CI réussi 🎉'
        }
        failure {
            echo 'Échec du pipeline ❌'
        }
    }
}
