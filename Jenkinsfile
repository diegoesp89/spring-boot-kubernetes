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
    stage('Initial') {
      steps {
        echo '========================================='
        echo '                INITIAL '
        echo '========================================='
        sh ''
        '
        echo "PATH = ${PATH}"
        echo "M2_HOME = ${M2_HOME}"
        ''
        '
      }
    }

    stage('Compile') {
      steps {
        echo '========================================='
        echo '                COMPILE '
        echo '========================================='
        sh 'mvn clean compile -e'
      }
    }
    stage('Test') {
      steps {
        echo '========================================='
        echo '                TEST '
        echo '========================================='
        sh 'mvn clean test -e'
      }
    }

    stage('SonarQube analysis') {
      steps {
        script {
          def scannerHome = tool 'SonarQube';
          withSonarQubeEnv('SonarQube') {
            sh "${scannerHome}/bin/sonar-scanner -Dsonar.java.binaries=/var/snap/jenkins/2126/workspace/tareadevsecops_master/target/classes -Dsonar.projectKey=ejerciciodevop -Dsonar.host.url=http://localhost:9000 -Dsonar.login=665ee488b3cf9d3190b0c9f0f81fa261cbe075fe"

          }
        }
      }
    }

    stage('SCA') {
      steps {
        sh 'mvn org.owasp:dependency-check-maven:check'
        dependencyCheckPublisher failedNewCritical: 5, failedTotalCritical: 10, pattern: 'target/dad.xml', unstableNewCritical: 3, unstableTotalCritical: 5
      }
    }

    stage('ZAP') {
      steps {
        script {
          env.DOCKER = tool "Docker"
          env.DOCKER_EXEC = "${DOCKER}/bin/docker"
          env.TARGET = 'http://zero.webappsecurity.com'

          sh '${DOCKER_EXEC} rm -f zap2'
          sh '${DOCKER_EXEC} pull owasp/zap2docker-stable'
          sh '${DOCKER_EXEC} run --add-host="localhost:192.168.100.4" --rm -e LC_ALL=C.UTF-8 -e LANG=C.UTF-8 --name zap2 -u zap -p 8090:8080 -d owasp/zap2docker-stable zap.sh -daemon -port 8080 -host 0.0.0.0 -config api.disablekey=true'
          sh '${DOCKER_EXEC} run --add-host="localhost:192.168.100.4" -v /Users/asajuro/Documents/BCI/AnalyzeQAS/Jenkins-Practica/USACH/Dockerfile/zap/jenkins_home/tools:/zap/wrk/:rw --rm -i owasp/zap2docker-stable zap-baseline.py -t "https://demo.testfire.net/" -I -r zap_baseline_report2.html -l PASS'
        }
      }
    }

    stage('Publish') {
      steps {
        publishHTML([
          allowMissing: false,
          alwaysLinkToLastBuild: false,
          keepAll: false,
          reportDir: '/var/jenkins_home/tools',
          reportFiles: 'zap_baseline_report2.html',
          reportName: 'HTML Report',
          reportTitles: ''
        ])
      }
    }

   

  }
}



