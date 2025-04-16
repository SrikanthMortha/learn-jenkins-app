pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            environment {
                HOME = "${WORKSPACE}/.jenkins-home"
                NPM_CONFIG_CACHE = "${WORKSPACE}/.npm"
            }
            steps {
                sh '''
                    mkdir -p $HOME $NPM_CONFIG_CACHE
                    ls -la
                    node --version
                    npm --version
                    npm ci --cache=$NPM_CONFIG_CACHE
                    npm run build
                    ls -la
                '''
            }
        }

        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            environment {
                NPM_CONFIG_CACHE = "${WORKSPACE}/.npm"
            }
            steps {
                sh '''
                    echo ">>> Running unit tests..."
                    npm test --cache=$NPM_CONFIG_CACHE
                '''
            }
        }
    }
}
