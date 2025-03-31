pipeline {
    agent any

    environment{
        NETLIFY_SITE_ID = "2640447c-4465-4efc-bfba-98011c547a6b"
        NETLIFY_AUTH_TOKEN = credentials('NETLIFY_AUTH_TOKEN')
    }

    stages {
        stage('build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                 ls -la
                 node --version
                 npm --version
                 npm ci
                 npm run build
                 ls -la
                '''
            }
           
        }
        stage('Deploy'){
            agent {
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps{
                sh '''
                  npm install netlify-cli
                  ls -la
                  node_modules/.bin/netlify --version
                  echo "deploying to provide: siteid: $NETLIFY_SITE_ID"
                  node_modules/.bin/netlify status
                  node_modules/.bin/netlify deploy --dir=build --prod
                '''
            }
        }
    }
}