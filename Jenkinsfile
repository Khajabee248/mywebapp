
pipeline {
  agent { label 'slave-node-1' } 
  environment {
    TOMCAT_PATH = '/opt/tomcat9'
    WAR_NAME = 'mywebapp.war'
  }
  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }
    stage('Build') {
      steps {
        sh 'mvn -B -DskipTests clean package'
      }
    }
    stage('Deploy to Tomcat') {
      steps {
        sh '''
          sudo mv -f target/${WAR_NAME} ${TOMCAT_PATH}/webapps/
          sudo chown -R tomcat:tomcat ${TOMCAT_PATH}/webapps/${WAR_NAME}
          sudo chmod 644 ${TOMCAT_PATH}/webapps/${WAR_NAME}
          sudo /bin/bash -c "${TOMCAT_PATH}/bin/shutdown.sh || true"
          sleep 2
          sudo /bin/bash -c "${TOMCAT_PATH}/bin/startup.sh"
        '''
      }
    }
  }
}
