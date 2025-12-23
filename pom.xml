pipeline {
    agent any

    // Définition des outils configurés dans Jenkins
    tools {
        maven 'M2_HOME'   // Nom de ton Maven dans Jenkins
        jdk 'JAVA_HOME'    // Nom de ton JDK dans Jenkins
    }

    stages {
        stage('Checkout SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/Tasnim70/MonProjetMaven.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'mvn test'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                // Utilisation sécurisée du token SonarQube
                withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
                    withSonarQubeEnv('sqsd') {  // Nom de ton serveur SonarQube configuré dans Jenkins
                        sh 'mvn sonar:sonar -Dsonar.login=$SONAR_TOKEN'
                    }
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
    }

    post {
        success {
            echo "Le pipeline s'est terminé avec succès ✅"
        }
        failure {
            echo "Le pipeline a échoué ❌"
        }
    }
}
