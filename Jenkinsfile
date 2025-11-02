pipeline {
    agent { label 'slave-node-1' }   // ✅ Runs on slave node

    environment {
        TOMCAT_HOME = '/opt/tomcat9'
        WAR_FILE = 'target/mywebapp.war'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Fetching code from Git...'
                git branch: 'main', 
                    url: 'https://github.com/Khajabee248/mywebapp.git',
                    credentialsId: 'b0d750c5-17f2-4a67-872c-d28f92b71329'
            }
        }

        stage('Build') {
            steps {
                echo 'Building Maven project...'
                sh '''
                    export PATH=$PATH:/usr/share/maven/bin
                    mvn clean package -DskipTests
                '''
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                echo 'Deploying WAR file to Tomcat server...'
                // ✅ No sudo password prompt
                sh '''
                    sudo cp $WAR_FILE $TOMCAT_HOME/webapps/
                    sudo systemctl restart tomcat9
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Deployment successful! App deployed to Tomcat.'
        }
        failure {
            echo '❌ Deployment failed. Please check logs.'
        }
    }
}
