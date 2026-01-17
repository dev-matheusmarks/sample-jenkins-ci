pipeline {
  agent any

  environment {
    IMAGE = "sample-python-ci:${env.BUILD_NUMBER}"
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
        sh 'git rev-parse --short HEAD'
      }
    }

    stage('Build') {
      steps {
        sh 'make build'
      }
    }

    stage('Unit Tests') {
      steps {
        sh 'make test'
      }
    }

    stage('Build Docker Image') {
      steps {
        sh 'make build-docker IMAGE=${IMAGE}'
      }
    }
  }

  post {
    always {
      script {
        if (isUnix()) {
          sh 'docker images | head -n 20 || true'
        } else {
          bat 'docker images | findstr /V "^$" || exit /b 0'
        }
      }
    }
  }
}
