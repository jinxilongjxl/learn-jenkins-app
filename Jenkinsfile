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

        stage("Tests") {
            parallel {
                stage('Unit Test') {
                    steps{
                        sh '''
                            test -f build/index.html
                            npm test
                        '''
                    }
                    post{
                        always {
                            junit 'jest-results/junit.xml'
                        }
                    }
                }

                stage('E2E') {
                    steps{
                        sh '''
                            npm install serve
                            node_modules/.bin/serve -s build &
                            sleep 10
                            # npx playwright install
                            npx playwright test --reporter=html
                        '''
                    }
                    post{
                        always {
                            publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, icon: '', keepAll: false, reportDir: 'playwright-report', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: '', useWrapperFileDirectly: true])
                        }
                    }
            }
        }
    }
}
