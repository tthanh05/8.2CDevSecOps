pipeline {
  agent any
  tools { nodejs 'node18' }  // uses NodeJS plugin auto-install

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main',
            url: 'https://github.com/tthanh05/8.2CDevSecOps.git',
            credentialsId: 'gh-pat'   // remove if repo is public
      }
    }
    stage('Install Dependencies') { steps { sh 'npm install' } }
    stage('Run Tests')            { steps { sh 'npm test || true' } }
    stage('Coverage')             { steps { sh 'npm run coverage || true' } }
    stage('NPM Audit')            { steps { sh 'npm audit || true' } }
  }

  post {
    always {
      archiveArtifacts artifacts: '**/coverage*/**', allowEmptyArchive: true
    }
  }
}
