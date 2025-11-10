pipeline {
  agent any

  environment {
    COMPOSE_FILE = "docker-compose-ci.yml"
    PROJECT_NAME = "flask_postgres_ci"
  }

  stages {

    stage('Checkout') {
      steps {
        echo "Cloning repository..."
        checkout scm
      }
    }

    stage('Run CI Compose (Build & Run)') {
      steps {
        script {
          echo "Docker and Compose versions:"
          sh "docker --version"
          sh "docker compose version"

          echo "Starting CI containers using docker-compose..."
          // Clean up any previous containers to avoid conflicts
          sh "docker compose -f ${COMPOSE_FILE} down -v || true"

          // Build and start in detached mode
          sh "docker compose -f ${COMPOSE_FILE} up -d --remove-orphans --build"

          echo "CI containers are up and running!"
          sh "docker ps"
        }
      }
    }

    stage('Smoke Check') {
      steps {
        script {
          echo "Waiting for web service to initialize..."
          sh "sleep 8"

          echo "Running smoke test inside web container..."
          // Simple smoke test to ensure Python environment is working
          sh """
          docker exec -i myapp_web_ci bash -c 'python3 -c "import sys; print(\\\"PYTHON OK\\\")"'
          """
        }
      }
    }

    stage('Tear Down CI env') {
      steps {
        script {
          echo "Stopping and cleaning up CI environment..."
          sh "docker compose -f ${COMPOSE_FILE} down -v"
        }
      }
    }
  }

  post {
    success {
      echo "✅ CI Pipeline completed successfully!"
    }
    failure {
      echo "❌ CI Pipeline failed. Check above logs for details."
    }
    always {
      echo "Pipeline finished."
      sh "docker system prune -f || true"
    }
  }
}
