pipeline {
  agent any
  environment {
    COMPOSE_FILE = "docker-compose-ci.yml"
  }
  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Run CI Compose (build & run)') {
      steps {
        script {
          sh "docker --version || true"
          // start CI compose (detached)
          sh "docker compose -f ${COMPOSE_FILE} up -d --remove-orphans --build"
        }
      }
    }

    stage('Smoke Check') {
      steps {
        sh "sleep 6"
        sh "docker exec -i myapp_web_ci bash -c 'python3 -c \"import sys; print(\\\"PYTHON OK\\\")\"' || true"
      }
    }

    stage('Tear Down CI env') {
      steps {
        sh "docker compose -f ${COMPOSE_FILE} down -v"
      }
    }
  }
  post {
    always {
      echo 'Pipeline finished'
    }
  }
}
