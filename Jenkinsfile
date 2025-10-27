pipeline {
    agent any

    tools {
        maven 'Maven'   // the name you configured under Jenkins → Global Tool Configuration
        jdk 'JDK21'     // change if your JDK name is differentad
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/neelkhot/witsolapur-webapp.git'
            }
        }

        stage('Build with Maven') {
            steps {
                bat 'mvn clean package'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                deploy adapters: [
                    tomcat9(
                        credentialsId: 'tomcat-creds1',  // must match the credentials ID in Jenkins
                        path: '',
                        url: 'http://localhost:8080'
                    )
                ], contextPath: '/witsolapur', war: 'target/witsolapur-webapp.war'

            }
        }
    }

    post {
        success {
            echo '✅ Application built and deployed successfully!'
            echo 'Access it at: http://localhost:8080/witsolapur'
        }
        failure {
            echo '❌ Build or deployment failed. Check logs for details.'
        }
    }
}
