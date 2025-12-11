pipeline {
    agent any
    tools {
        nodejs 'NodeJS_24'
    }
    stages {
        stage("Build") {
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

        stage('Test') {
            steps{
                sh '''
                    test -f build/index.html
                    npm test
                '''
            }
        }

        stage('E2E') {
            steps{
                sh '''
                    npm install serve
                    // don't block, run serve in background
                    node_modules/.bin/serve -s build &
                    // wait for serve to start
                    sleep 10
                    npx playwright test
                '''
            }
        }

    }

    post{
        always {
            junit 'test-results/junit.xml'
        }
    }
}
