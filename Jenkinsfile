pipeline {
    agent { label 'slave1' }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/jaykasundra2810/medicare-devops-project.git'
            }
        }
        stage('Build') {
            steps {
                sh './mvnw clean package'
            }
        }
        stage('Test') {
            steps {
                sh './mvnw test'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t medicare-microservice:latest .'
            }
        }
        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh 'docker login -u $USER -p $PASS'
                    sh 'docker tag medicare-microservice:latest jayk2810/medicare-microservice:latest'
                    sh 'docker push jayk2810/medicare-microservice:latest'
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s-deployment.yml'
            }
        }
    }
}
