pipeline {
    agent { label 'slave-node-1' }  // your slave node label

    environment {
        TOMCAT_URL = "http:54.196.80.141:8080"
        TOMCAT_USER = "tomcat"
        TOMCAT_PASS = "admin"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/YOUR_USER/mywebapp.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                sh """
                    curl -u ${TOMCAT_USER}:${TOMCAT_PASS} \\
                    --upload-file target/mywebapp.war \\
                    "${TOMCAT_URL}/manager/text/deploy?path=/mywebapp&update=true"
                """
            }
        }
    }

    post {
        success {
            echo '✅ Deployment successful!'
        }
        failure {
            echo '❌ Deployment failed.'
        }
    }
}

