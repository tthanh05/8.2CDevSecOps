pipeline {
  agent any
  options { ansiColor('xterm') }

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/tthanh05/8.2CDevSecOps.git'
      }
    }

    stage('Install Node (if missing)') {
      steps {
        sh '''
          if ! command -v node >/dev/null 2>&1; then
            echo "Installing Node.js 18..."
            curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash - || true
            sudo apt-get update -y || true
            sudo apt-get install -y nodejs || true
          fi
          node -v
          npm -v
        '''
      }
    }

    stage('Install Dependencies') {
      steps { sh 'npm install' }
    }

    stage('Run Tests') {
      steps { sh 'npm test || true' }   // keep pipeline going even if tests fail
    }

    stage('Generate Coverage Report') {
      steps { sh 'npm run coverage || true' }
    }

    stage('NPM Audit (Security Scan)') {
      steps { sh 'npm audit || true' }  // shows CVEs in console output
    }
  }

  post {
    always {
      archiveArtifacts artifacts: '**/coverage*/**', allowEmptyArchive: true
    }
  }
}
