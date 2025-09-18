pipeline {
  agent any
  options { timestamps() }
  triggers { pollSCM('H/5 * * * *') } // auto-check for commits

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/5028519-Pisey/8.2CDevSecOps.git'
      }
    }
    stage('Install Dependencies') {
      steps {
        sh 'test -f package-lock.json && npm ci || npm install'
      }
    }
    stage('Run Tests') {
      steps {
        sh 'npm test || echo "No test script or tests failed — continuing"'
      }
    }
    stage('Generate Coverage Report') {
      steps {
        sh 'npm run coverage || echo "No coverage script — continuing"'
      }
    }
    stage('NPM Audit (Security Scan)') {
      steps {
        sh 'npm audit || true'
      }
    }
  }
  post {
    always {
      echo "Pipeline finished with result: ${currentBuild.currentResult}"
    }
  }
}
