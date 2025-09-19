pipeline {
  agent { docker { image 'node:18' } }   // runs inside Node 18 container
  stages {
    stage('Checkout') {
      steps {
        git branch: 'main',
            url: 'https://github.com/tthanh05/8.2CDevSecOps.git',
            credentialsId: 'gh-pat'   // remove if repo is public
      }
    }
    stage('Install Deps')   { steps { sh 'npm ci || npm install' } }
    stage('Run Tests')      { steps { sh 'npm test || true' } }
    stage('Coverage')       { steps { sh 'npm run coverage || true' } }
    stage('NPM Audit')      { steps { sh 'npm audit || true' } }
  }
  post { always { archiveArtifacts artifacts: '**/coverage*/**', allowEmptyArchive: true } }
}
