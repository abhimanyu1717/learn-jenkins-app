pipeline {
  agent any
  environment {
    NETLIFY_SITE_ID ='39054246-db54-4544-bd60-bedb6287a1ec'
    NETLIFY_AUTH_TOKEN = credentials('netlify-token')
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
              echo "Added comment to check Poll scm command..."
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
      stage('Deployment Staging') {
        agent{
            docker {
                image 'my-playwrigh'
                reuseNode true
            }
        }
        steps {
          sh '''
            echo "******** Netlify version*******"
            echo "Deploying to Staging Site Id:$NETLIFY_SITE_ID"
            netlify status
            netlify deploy --dir=build

          '''
        }
      }
      stage('Approval for Prod Env...') {
                steps {
                    timeout(5) {
                        input message: 'Please approve deployment?', ok: 'Approved for Prod Deployment'
                      }
                  
                }
            }
      stage('Deployment prod') {
        agent{
            docker {
                image 'my-playwrigh'
                reuseNode true
            }
        }
        steps {
          sh '''
            echo "Deploying to production Site Id:$NETLIFY_SITE_ID"
             netlify status
             netlify deploy --dir=build --prod

          '''
        }
      }
  
  }

}
