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
                step {
                    name: 'Run Unit Tests'
                    script {
                        sh '''
                            test -f build/index.html
                            npm run test:ci
                        '''
                    }
            }
        }
    }
}
