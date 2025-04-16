pipeline {
    agent any

    stages {
        stage('Build with Node.js') {
            agent {
                docker {
                    image 'node:18'         // Full Node image, not Alpine
                    args '-u root'          // Run as root to avoid permission issues
                    reuseNode true
                }
            }

            steps {
                sh '''
                    set -e  # Fail fast

                    echo "🔍 Checking workspace contents"
                    ls -la

                    echo "📦 Checking Node & npm"
                    node --version
                    npm --version

                    echo "📥 Installing dependencies"
                    npm install

                    echo "🚧 Checking and running build if script exists"
                    if npm run | grep -q build; then
                        npm run build
                    else
                        echo "⚠️ No build script found in package.json"
                    fi

                    echo "✅ Done. Final workspace:"
                    ls -la
                '''
            }
        }
    }
}
