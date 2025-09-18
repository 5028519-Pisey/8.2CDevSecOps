pipeline {
  agent any

  options { timestamps() }

  environment {
    RECIPIENTS = 'piseypich45@gmail.com'   
  }

  stages {
    stage('Build') {
      steps {
        sh 'echo building...'
      }
    }

    stage('Tests') {
      steps {
        sh 'echo running tests...'
        // Example artifact to attach later
        sh 'echo "report content" > report.txt'
        archiveArtifacts artifacts: 'report.txt', onlyIfSuccessful: false
      }
    }
  }

  post {
    always {
      script {
        emailext(
          to: "${env.RECIPIENTS}",
          subject: "Jenkins ${env.JOB_NAME} #${env.BUILD_NUMBER} - ${currentBuild.currentResult}",
          body: """<p>Build: <b>${env.JOB_NAME} #${env.BUILD_NUMBER}</b></p>
                   <p>Status: <b>${currentBuild.currentResult}</b></p>
                   <p>URL: ${env.BUILD_URL}</p>""",
          mimeType: 'text/html',
          attachLog: true,                        // Attach console log
          attachmentsPattern: 'report.txt, **/coverage*.html' // Any artifacts/patterns
        )
      }
    }
  }
}
