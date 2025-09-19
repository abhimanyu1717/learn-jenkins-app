pipeline {
  agent any

  stages {
      stage('Build') {
        agent {
            docker {
                image 'node:18-alpine'
                reuseNode true
            }
        }
          steps {
              sh '''
              echo "List of all file before build............"
              ls -la
              echo 'Node and npm version....'
              node --version
              npm --version
              npm ci
              npm run build
              echo "List of all file after bild......"
              ls -la
              '''
          }
      }
  }
}