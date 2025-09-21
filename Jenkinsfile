pipeline {
  agent any
  environment {
    NETLIFY_SITE_ID ='39054246-db54-4544-bd60-bedb6287a1ec'
  }
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
            npm install netlify-cli@20.1.1
            echo "******** Netlify version*******"
            node_modules/.bin/netlify --version
            echo "Deploying to production Site Id:$NETLIFY_SITE_ID"

          '''
        }
      }
  
  }

}