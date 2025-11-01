pipeline {
    agent any

    environment {
        TOMCAT_HOME = '/opt/tomcat9'
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Fetching code from Git...'
                git branch: 'main',
                    credentialsId: 'b0d750c5-17f2-4a67-872c-d28f92b71329',
                    url: 'https://github.com/Khajabee248/mywebapp.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Building Maven project...'
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                echo 'Deploying WAR file to Tomcat server...'
                sh '''
                    # Stop Tomcat if running (ignore errors)
                    sudo ${TOMCAT_HOME}/bin/shutdown.sh || true

                    # Copy new WAR to webapps
                    sudo cp target/mywebapp.war ${TOMCAT_HOME}/webapps/

                    # Start Tomcat again
                    sudo ${TOMCAT_HOME}/bin/startup.sh
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Deployment successful! App is live on Tomcat.'
        }
        failure {
            echo '❌ Deployment failed. Please check logs.'
        }
    }
}

