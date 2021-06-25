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
    stage('SonarQube analysis') {
           steps{
                script {
                    def scannerHome = tool 'SonarQube';
                    withSonarQubeEnv('Sonar Server') {
                      sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=ejerciciodevop -Dsonar.host.url=http://localhost:9000 -Dsonar.login=665ee488b3cf9d3190b0c9f0f81fa261cbe075fe"

                    }
                }
           }
        }


     
    }
}



