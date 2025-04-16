pipeline {
    agent any

    stages {
        stage('Build with Node.js') {
            agent {
                docker {
                    image 'node:18'         // Full Node image (not Alpine)
                    reuseNode true
                }
            }

            steps {
                sh '''
                    echo "🔍 Workspace contents:"
                    ls -la

                    echo "📦 Node & npm versions:"
                    node --version
                    npm --version

                    echo "📥 Installing dependencies..."
                    npm install || true    # Avoid hard stop if it fails

                    echo "🚧 Running build (if available)..."
                    if npm run | grep -q build; then
                      npm run build
                    else
                      echo "⚠️ No build script found in package.json"
                    fi

                    echo "✅ Final workspace status:"
                    ls -la
                '''
            }
        }
    }
}
