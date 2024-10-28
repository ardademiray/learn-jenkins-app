pipeline {
    agent any

    stages {
        /*
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
        */
        stage ('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            
            steps {
                sh '''
                    echo "Step 1: Check if index.html file exist"
                    #test -f build/index.html
                    echo "Step 2: Run nmp test command"
                    npm test
                '''
            }
        }

        stage ('E2E') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.48.1-noble'
                    reuseNode true
                    args ''
                }
            }
            
            steps {
                sh '''
                    npm install serve
                    node_modules/.bin/serve -s build
                    npx playwright test
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