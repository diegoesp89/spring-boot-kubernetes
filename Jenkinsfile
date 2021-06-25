pipeline {
    agent any

    tools {
        maven 'Maven'
    }

    stages {
    
    stage('Cloning Git') {
      steps {
        git url: 'https://github.com/diegoesp89/spring-boot-kubernetes.git',
            credentialsId: 'diegoesp89'
      }
    }
      stage ('Initial') {
            steps {
              echo '========================================='
              echo '                INITIAL '
              echo '========================================='
              sh '''
                   echo "PATH = ${PATH}"
                   echo "M2_HOME = ${M2_HOME}"
               '''
            }
        }
        
         stage ('Compile') {
            steps {
                echo '========================================='
                echo '                COMPILE '
                echo '========================================='
                 sh 'mvn clean compile -e'
            }
        }
        stage ('Test') {
            steps {
                echo '========================================='
                echo '                TEST '
                echo '========================================='
                 sh 'mvn clean test -e'
            }
        }

    stage('SonarQube analysis') {
           steps{
                script {
                    def scannerHome = tool 'SonarQube';
                    withSonarQubeEnv('SonarQube') {
                      sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=ejerciciodevop -Dsonar.host.url=http://localhost:9000 -Dsonar.login=665ee488b3cf9d3190b0c9f0f81fa261cbe075fe"

                    }
                }
           }
        }


     
    }
}



