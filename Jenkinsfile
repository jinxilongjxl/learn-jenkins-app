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
    }

    post{
        always {
            junit 'test-results/junit.xml'
        }
    }
}
