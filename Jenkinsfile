pipeline {

 agent any

 tools {jdk 'JAVA_HOME’, maven 'M2_HOME'}

 stages {

 stage('GIT') {

           steps {

               git branch: 'main',

               url: ' https://github.com/Tasnim70/MonProjetMaven.git'

          }

     }

 stage ('Compile Stage') {

 steps {

 sh 'mvn clean compile'

 }

 }

 }

}
