pipeline {
    agent { label 'slave-node-1' }

    stages {
        stage('Checkout') {
            steps {
                echo "Fetching code from Git..."
                git branch: 'main', url: 'https://github.com/Khajabee248/mywebapp.git', credentialsId: 'b0d750c5-17f2-4a67-872c-d28f92b71329'
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
                echo "Deploying WAR file to Tomcat server..."
                sh '''
                # Copy WAR file to Tomcat webapps folder
                sudo cp target/mywebapp.war /opt/tomcat9/webapps/

                # Restart Tomcat service
                sudo systemctl restart tomcat9

                # Verify
                sudo ls /opt/tomcat9/webapps/
                '''
            }
        }
    }
}

