pipeline {
  agent any
  options {
    timestamps()           // show times in console
    ansiColor('xterm')     // nicer log (optional)
  }

  environment {
    RECIPIENTS = 'piseypich45@gmail.com'
  }

  stages {
    stage('Demo') {
      steps {
        sh 'echo hello from Jenkins'
        // Example artifact to prove attachments work
        sh 'echo "sample report content" > report.txt'
        // Archive so emailext can find it via attachmentsPattern
        archiveArtifacts artifacts: 'report.txt', onlyIfSuccessful: false
      }
    }
  }

  post {
    // Send email after the whole pipeline finishes (success or fail)
    always {
      // Email Extension Plugin (emailext) must be installed & configured in Manage Jenkins → Configure System → Extended E-mail Notification
      emailext(
        // if you set Default Recipients in Jenkins global config, you may omit "to:"
        to: "${env.RECIPIENTS}",
        recipientProviders: [],                 // avoid auto-adding culprits/developers; use only what we pass
        subject: "Jenkins ${env.JOB_NAME} #${env.BUILD_NUMBER} - ${currentBuild.currentResult}",
        mimeType: 'text/html',
        body: """
          <p><b>Job:</b> ${env.JOB_NAME}</p>
          <p><b>Build #:</b> ${env.BUILD_NUMBER}</p>
          <p><b>Status:</b> <b>${currentBuild.currentResult}</b></p>
          <p><b>URL:</b> <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
          <p>Console log is attached. Any listed artifacts are attached too.</p>
        """,
        attachLog: true,                        // attach console log
        compressLog: true,                      // zip the log (smaller)
        attachmentsPattern: 'report.txt'        // add more with commas, e.g. 'report.txt, **/coverage*.html'
      )
    }
  }
}
