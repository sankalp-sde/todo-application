pipeline {
    agent any
    environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub-credentials')
    }
    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/<your-repo>'
            }
        }
        stage('Build with Maven') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t todo-application-image:latest .'
            }
        }
        stage('Push to Docker Hub') {
            steps {
                sh '''
                    echo $DOCKER_HUB_CREDENTIALS_PSW | docker login -u $DOCKER_HUB_CREDENTIALS_USR --password-stdin
                    docker tag todo-application-image:latest <dockerhub-username>/todo-application:latest
                    docker push <dockerhub-username>/todo-application:latest
                '''
            }
        }
        stage('Deploy with Docker Compose') {
            steps {
                sh 'docker-compose up -d'
            }
        }
    }
    post {
        success {
            sh 'rm -rf *'
        }
    }
}
