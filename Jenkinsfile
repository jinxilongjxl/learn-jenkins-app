pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = '16788fbf-9e43-4468-82ae-4cafa429b11d'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }

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
                stage('Test') {
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

        stage("Deploy") {
            steps {
                sh '''
                    npm install netlify-cli
                    node_modules/.bin/netlify --version
                    echo "Deploying to production. Netlify site ID: $NETLIFY_SITE_ID"
                    node_modules/.bin/netlify status
                '''
            }
        }

    }
}
