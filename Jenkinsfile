pipeline {
  agent any

  environment {
    APP_IMAGE = "astro-app:latest"
    APP_CONTAINER = "astro-app"
  }

  stages {
    stage("Checkout") {
      steps {
        checkout scm
      }
    }

    stage("Node info") {
      steps {
        sh "node -v"
        sh "npm -v"
      }
    }

    stage("Install") {
      steps {
        sh "npm ci"
      }
    }

    stage("Build Astro") {
      steps {
        sh "npm run build"
      }
    }

    stage("Docker build") {
      steps {
        sh "docker version"
        sh "docker build -t ${APP_IMAGE} ."
      }
    }

    stage("Deploy container") {
      steps {
        sh """
          docker rm -f ${APP_CONTAINER} || true
          docker run -d --name ${APP_CONTAINER} -p 3000:3000 ${APP_IMAGE}
        """
      }
    }
  }

  post {
    always {
      sh "docker ps --format 'table {{.Names}}\\t{{.Image}}\\t{{.Ports}}' | head -n 20 || true"
    }
  }
}
