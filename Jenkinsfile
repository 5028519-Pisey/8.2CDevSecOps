pipeline {
  agent any
  options { timestamps() }   

  environment {
    RECIPIENTS = 'piseypich45@gmail.com'
  }

  stages {
    stage('Demo') {
      steps {
        sh 'echo hello from Jenkins'
        sh 'echo "sample report content" > report.txt'
        archiveArtifacts artifacts: 'report.txt', onlyIfSuccessful: false
      }
    }
  }

  post {
    always {
      emailext(
        to: "${env.RECIPIENTS}",
        recipientProviders: [],   // use ONLY the explicit recipient(s)
        subject: "Jenkins ${env.JOB_NAME} #${env.BUILD_NUMBER} - ${currentBuild.currentResult}",
        mimeType: 'text/plain',
        body: "Build URL: ${env.BUILD_URL}",
        attachLog: true,
        compressLog: true,
        attachmentsPattern: 'report.txt'
      )
    }
  }
}
