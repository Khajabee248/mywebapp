pipeline {
  agent { label 'slave-node-1' }   // your slave node label name 

  stages {
    stage('Checkout') {
      steps {
        echo "Fetching code from Git..."
        checkout scm
      }
    }

    stage('Build') {
      steps {
        echo "Building Maven project..."
        sh 'mvn clean package -DskipTests'
      }
    }

    stage('Deploy to Tomcat') {
      steps {
        echo "Deploying WAR to Tomcat..."
        sh '''
          sudo systemctl stop tomcat9 || true
          sudo cp target/mywebapp.war /opt/tomcat9/webapps/
          sudo chown tomcat:tomcat /opt/tomcat9/webapps/mywebapp.war
          sudo systemctl start tomcat9
        '''
      }
    }
  }
}
