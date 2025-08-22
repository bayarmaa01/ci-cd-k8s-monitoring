pipeline {
    agent any
    environment {
        DOCKERHUB_USER = 'bayarmaa'                // your DockerHub username
        IMAGE_NAME     = 'ci-cd-lab'
    }
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/bayarmaa01/ci-cd-k8s-monitoring.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKERHUB_USER/$IMAGE_NAME:$BUILD_NUMBER .'
            }
        }
        stage('Push to DockerHub') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub-pass', variable: 'DOCKER_PASS')]) {
                    sh "echo $DOCKER_PASS | docker login -u $DOCKERHUB_USER --password-stdin"
                }
                sh 'docker push $DOCKERHUB_USER/$IMAGE_NAME:$BUILD_NUMBER'
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl set image deployment/ci-cd-app ci-cd-app=$DOCKERHUB_USER/$IMAGE_NAME:$BUILD_NUMBER'
            }
        }
    }
}
