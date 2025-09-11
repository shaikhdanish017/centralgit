pipeline {
    agent any

    tools {
        maven 'Maven-3.8.8'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/shaikhdanish017/centralgit.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                sh '''
                # copy WAR to Tomcat (assuming Tomcat on same server)
                cp target/*.war /var/lib/tomcat9/webapps/
                '''
            }
        }

        stage('Deploy to Docker') {
            steps {
                sh '''
                docker build -t centralgit:latest .
                docker run -d --name centralgit-app -p 8081:8080 centralgit:latest
                '''
            }
        }
    }
}
