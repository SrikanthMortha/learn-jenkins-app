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
            steps {
                sh '''
                    echo ">>> Checking if index.html exists in build directory..."
                    if [ -f build/index.html ]; then
                      echo "✅ index.html found."
                    else
                      echo "❌ index.html NOT found!"
                      exit 1
                    fi
                '''
            }
        }
    }
}
