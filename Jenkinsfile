pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        echo "Checking out..."
        checkout scm
      }
    }

    stage('Build on Slave') {
      agent { label 'slave-node-1' }
      steps {
        echo "Running mvn package on slave-node-1"
        sh 'mvn -B clean package'
      }
      post {
        success {
          archiveArtifacts artifacts: 'target/*.war', fingerprint: true
          junit 'target/surefire-reports/*.xml'
        }
      }
    }

    // Optional deploy stage (enable if you have Tomcat on the slave and proper permissions)
     
    stage('Deploy to Tomcat') {
      agent { label 'slave-node-1' }
      steps {
        echo "Deploying to Tomcat"
        sh '''
          sudo cp target/mywebapp.war /opt/tomcat9/webapps/
          sudo systemctl restart tomcat9
        '''
      }
    }
     
  }

  post {
    always {
      echo "Pipeline finished. Result: ${currentBuild.currentResult}"
    }
    failure {
      echo "Build failed!"
    }
  }
}
