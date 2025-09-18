pipeline {
  agent any

  environment {
    PATH = "/opt/homebrew/bin:/usr/local/bin:/usr/bin:/bin:${env.PATH}"
  }

  options { timestamps() }

  stages {
    stage('Checkout') {
      steps { git branch: 'main', url: 'https://github.com/5028519-Pisey/8.2CDevSecOps.git' }
    }

    stage('Install Dependencies') {
      steps {
        sh '''
          which node || true
          which npm || true
          test -f package-lock.json && npm ci || npm install
        '''
      }
    }

    stage('Run Tests') {
      steps { sh 'npm test || echo "No test script or tests failed — continuing"' }
    }

    stage('Generate Coverage Report') {
      steps { sh 'npm run coverage || echo "No coverage script — continuing"' }
    }

    stage('NPM Audit (Security Scan)') {
      steps { sh 'npm audit || true' }
    }
  }

  post { always { echo "Pipeline finished with result: ${currentBuild.currentResult}" } }
}

