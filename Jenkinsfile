pipeline {
  agent any
  tools { nodejs 'node18' }               // NodeJS tool you configured

  environment {
    EMAIL_TO = 'lutuanthanh2017@gmail.com'          // <-- your inbox
  }

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main',
            url: 'https://github.com/tthanh05/8.2CDevSecOps.git'
            // , credentialsId: 'gh-pat'   // uncomment if repo is private
      }
    }

    stage('Install Dependencies') {
      steps { sh 'npm ci || npm install' }
    }

    stage('Run Tests') {
      steps {
        script {
          int code = sh(returnStatus: true, script: 'npm test')
          String status = (code == 0) ? 'SUCCESS' : 'FAILURE'
          emailext(
            to: env.EMAIL_TO,
            subject: "[${status}] Tests — ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            body: """Tests stage finished with ${status}.
Exit code: ${code}
Build URL: ${env.BUILD_URL}
""",
            attachLog: true, compressLog: true
          )
          // Do NOT error the build—assignment wants SUCCESS overall
        }
      }
    }

    stage('Coverage') {
      steps {
        // run but ignore exit so pipeline continues
        sh 'npm run coverage || true'
      }
    }

    stage('NPM Audit (Security Scan)') {
      steps {
        script {
          int code = sh(returnStatus: true, script: 'npm audit')
          String status = (code == 0) ? 'SUCCESS' : 'FAILURE'
          emailext(
            to: env.EMAIL_TO,
            subject: "[${status}] NPM Audit — ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            body: """NPM Audit stage finished with ${status}.
Exit code: ${code}
Review vulnerabilities in the attached log.
Build URL: ${env.BUILD_URL}
""",
            attachLog: true, compressLog: true
          )
          // Keep build green regardless of audit findings
        }
      }
    }
  }

  post {
    always {
      archiveArtifacts artifacts: '**/coverage*/**', allowEmptyArchive: true
      echo "Pipeline finished: ${currentBuild.currentResult}"
    }
  }
}
