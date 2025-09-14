pipeline {
    agent any

    tools {
        maven 'Maven-3.8.8'
    }

    environment {
        DOCKER_IMAGE = "shaikhdanish017/centralgit"
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
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                echo "Building Docker Image..."
                docker build -t $DOCKER_IMAGE:${BUILD_NUMBER} .
                docker tag $DOCKER_IMAGE:${BUILD_NUMBER} $DOCKER_IMAGE:latest
                '''
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub-pass', variable: 'DOCKERHUB_PASS')]) {
                    sh '''
                    echo "$DOCKERHUB_PASS" | docker login -u shaikhdanish017 --password-stdin
                    docker push $DOCKER_IMAGE:${BUILD_NUMBER}
                    docker push $DOCKER_IMAGE:latest
                    '''
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''
                echo "Deploying with Docker..."
                docker rm -f centralgit-app || true
                docker run -d --name centralgit-app -p 8081:8080 $DOCKER_IMAGE:${BUILD_NUMBER}
                '''
            }
        }
    }
}
