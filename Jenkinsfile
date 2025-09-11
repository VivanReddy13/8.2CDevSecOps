pipeline {
    agent any

    environment {
        NODE_ENV = 'development'
        SNYK_TOKEN = credentials('snyk-token') // Add Snyk token in Jenkins Credentials
        NODE_HOME = tool name: 'NodeJS 22.19', type: 'NodeJS'
        PATH = "${env.NODE_HOME}/bin:${env.PATH}"
    }

    stages {
        stage('Clean Workspace') {
            steps {
                echo "=== CLEANING WORKSPACE ==="
                deleteDir() // Removes all files from previous builds
            }
        }

        stage('Checkout') {
            steps {
                echo "=== CHECKOUT STAGE ==="
                git branch: 'main', url: 'https://github.com/VivanReddy13/8.2CDevSecOps.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                echo "=== INSTALL DEPENDENCIES ==="
                // Use npm cache for faster installs
                sh 'npm ci --cache .npm_cache'
            }
        }

        stage('Run Tests & Coverage') {
            steps {
                echo "=== TESTS & COVERAGE ==="
                sh 'npm run test:coverage'
            }
        }

        stage('Security Scan') {
            steps {
                echo "=== SECURITY SCAN ==="
                sh 'snyk auth $SNYK_TOKEN'
                sh 'snyk test'
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
