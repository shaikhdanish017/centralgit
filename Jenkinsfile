pipeline {
    agent any

    stages {
        stage('Clone Repo') {
    steps {
        git branch: 'master',
            url: 'https://github.com/shaikhdanish017/centralgit.git',
            credentialsId: 'github-creds'
           }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t myapp:${env.BUILD_NUMBER} ."
                sh "docker tag myapp:${env.BUILD_NUMBER} myapp:latest"
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                docker stop myapp || true
                docker rm myapp || true
                docker run -d --name myapp -p 5000:5000 myapp:latest
                '''
            }
        }
    }
}
