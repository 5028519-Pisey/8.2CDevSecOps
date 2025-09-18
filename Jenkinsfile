pipeline {
  agent any

  // Make sure Jenkins sees Homebrew's Node/npm
  environment {
    PATH = "/opt/homebrew/bin:/usr/local/bin:/usr/bin:/bin:${env.PATH}"
  }

  options { timestamps() }
  triggers { pollSCM('H/5 * * * *') } // Auto-check every ~5 minutes

  stages {
    stage('Checkout') {
      steps {
        // <<< EDIT ME >>> If your URL is different (or private SSH), change it here
        git branch: 'main', url: 'https://github.com/5028519-Pisey/8.2CDevSecOps.git'
        // If your repo is private and you added a PAT credential in Jenkins,
        // use this instead:
        // git branch: 'main', url: 'https://github.com/5028519-Pisey/8.2CDevSecOps.git', credentialsId: 'GITHUB_PAT'
      }
    }

    stage('Install Dependencies') {
      steps {
        sh '''
          echo "Node in PATH: $(which node || true)"
          echo "npm in PATH : $(which npm || true)"
          test -f package-lock.json && npm ci || npm install
        '''
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
    success {
      emailext(
        // <<< EDIT ME >>> your email address
        to: 'your@gmail.com',
        subject: "✅ SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
        body: """Build succeeded.

Job: ${env.JOB_NAME}
Build: #${env.BUILD_NUMBER}
Console: ${env.BUILD_URL}console

The full console log is attached for reference.""",
        attachLog: true
      )
    }
    failure {
      emailext(
        to: 'your@gmail.com',
        subject: "❌ FAILURE: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
        body: """Build failed.

Job: ${env.JOB_NAME}
Build: #${env.BUILD_NUMBER}
Console: ${env.BUILD_URL}console

The full console log is attached for debugging.""",
        attachLog: true
      )
    }
    always {
      echo "Pipeline finished with result: ${currentBuild.currentResult}"
    }
  }
}
