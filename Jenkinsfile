pipeline {
    agent any

    environment {
        TEST_RESULTS_DIR = "test-results"
    }

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    args '-v $WORKSPACE:/app'  // mount whole workspace
                    reuseNode true
                }
            }
            steps {
                sh '''
                    cd /app
                    npm ci
                    npm run build
                '''
            }
        }

        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    args '-v $WORKSPACE:/app'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    cd /app
                    mkdir -p test-results
                    npm test -- --reporters=jest-junit --outputFile=test-results/junit.xml
                    ls -la test-results
                '''
            }
        }
    }
}
