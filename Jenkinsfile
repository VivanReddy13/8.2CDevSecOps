pipeline {
    agent any

    environment {
        NODE_ENV = 'development'
        SNYK_TOKEN = credentials('snyk-token') // Add your Snyk token in Jenkins Credentials
    }

    stages {
        stage('Checkout') {
            steps {
                echo "=== CHECKOUT STAGE ==="
                git branch: 'main', url: 'https://github.com/your-repo.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                echo "=== INSTALL DEPENDENCIES ==="
                sh 'npm install'
            }
        }

        stage('Run Tests & Coverage') {
            steps {
                echo "=== TESTS & COVERAGE ==="
                // Ensure package.json has a script: "test:coverage": "jest --coverage"
                sh 'npm run test:coverage'
            }
        }

        stage('Security Scan') {
            steps {
                echo "=== SECURITY SCAN ==="
                // Snyk scan
                sh 'snyk auth $SNYK_TOKEN'
                sh 'snyk test'
                
                // NPM audit as fallback
                sh 'npm audit --audit-level=high'
            }
        }
    }

    post {
        always {
            echo "=== PIPELINE FINISHED ==="
        }
        failure {
            echo "Pipeline failed. Check logs for details."
        }
    }
}
