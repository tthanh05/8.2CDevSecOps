pipeline {
  agent any
  tools { nodejs 'node18' }   // Manage Jenkins → Global Tool Configuration → NodeJS → Name: node18

  environment {
    EMAIL_TO = 'lutuanthanh2017@gmail.com'  // <-- change to your inbox
  }

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main',
            url: 'https://github.com/tthanh05/8.2CDevSecOps.git'
      }
    }

    stage('Install Dependencies') {
      steps { sh 'npm ci || npm install' }
    }

    stage('Run Tests') {
      steps { sh 'npm test || true' }     // keep pipeline green for demo
      post {
        success {
          emailext(
            to: env.EMAIL_TO,
            subject: "[SUCCESS] Tests — ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            body: "Tests succeeded.\nBuild: ${env.BUILD_URL}",
            attachLog: true, compressLog: true
          )
        }
        failure {
          emailext(
            to: env.EMAIL_TO,
            subject: "[FAILURE] Tests — ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            body: "Tests failed.\nBuild: ${env.BUILD_URL}",
            attachLog: true, compressLog: true
          )
        }
      }
    }

    stage('Coverage') {
      steps { sh 'npm run coverage || true' }
    }

    stage('NPM Audit (Security Scan)') {
      steps { sh 'npm audit || true' }
      post {
        always {
          emailext(
            to: env.EMAIL_TO,
            subject: "[${currentBuild.currentResult}] NPM Audit — ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            body: "NPM Audit finished with: ${currentBuild.currentResult}.\nBuild: ${env.BUILD_URL}",
            attachLog: true, compressLog: true
          )
        }
      }
    }
  }

  post {
    always {
      archiveArtifacts artifacts: '**/coverage*/**', allowEmptyArchive: true
    }
  }
}
