pipeline {
    agent any
    environment {
        NETLIFY_SITE_ID = 'd0bd2541-f303-4bda-aa49-09b1ad091a98'
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
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la                    
                '''
            }
        }
        stage('Test'){
            agent {
                docker {
                    image  'node:18-alpine'
                    reuseNode true
                }
            }
            steps{
                sh '''
                    ls -la
                    echo "Test stage working fine"
                    npm test
                '''

            }
        }
        stage('Deploy') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm install netlify-cli               
                    node_modules/.bin/netlify --version
                    echo "Deploying to production environment with Site ID: $NETLIFY_SITE_ID"
                '''
            }
        }
    }

    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
}
