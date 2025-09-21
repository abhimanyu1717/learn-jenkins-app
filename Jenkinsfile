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
              echo "List of all file after build......"
              ls -la
              '''
          }
      }
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
            npm test
          '''
        }
      }
      stage('Deployment') {
        agent{
            docker {
                 image 'node:18-alpine'
                reuseNode true
            }
        }
        steps {
          sh '''
            npm install netlify-cli
            echo "******** Netlify version*******"
            node-modules/.bin/netlify --version
          '''
        }
      }
  
  }

}