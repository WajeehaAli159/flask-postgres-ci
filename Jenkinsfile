pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Cloning repository...'
                git branch: 'main', url: 'https://github.com/WajeehaAli159/flask-postgres-ci.git'
            }
        }

        stage('Build Containers') {
            steps {
                echo 'Building and running containers...'
                sh 'docker compose down || true'
                sh 'docker compose up -d'
            }
        }

        stage('Verify Containers') {
            steps {
                echo 'Checking running containers...'
                sh 'sleep 10 && docker ps'
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            sh 'docker compose down'
        }
    }
}

