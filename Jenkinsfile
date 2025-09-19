pipeline {
  agent any

  stages {
    stage('Test') {
        agent{
            docker {
                 image 'node:18-alpine'
                reuseNode true
            }
        }
        steps {
          sh '''
            test -f build/index.html
            echo 'Node and npm version....'
            node --version
            npm --version
            npm ci
            npm test
          '''
        }
      }
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
              npm run build
              echo "List of all file after build......"
              ls -la
              '''
          }
      }
      
  }
}