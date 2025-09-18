pipeline {
  agent any
  options { timestamps() }

  stages {
    stage('Demo') {
      steps {
        sh 'echo hello from Jenkins'
      }
    }
  }

  post {
    always {
      emailext(
        to: 'piseypich45@gmail.com',
        subject: "Build ${env.JOB_NAME} #${env.BUILD_NUMBER} - ${currentBuild.currentResult}",
        body: "Build URL: ${env.BUILD_URL}",
        mimeType: 'text/plain',
        attachLog: true,      // <-- attaches console log
        compressLog: true     // optional: zip the log
        // attachmentsPattern: 'report.txt, **/coverage*.html'  // optional: attach artifacts
      )
    }
  }
}
