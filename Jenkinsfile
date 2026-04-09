pipeline {
    agent { label 'docker-agent' }

    environment {
        APP_NAME = 'parallel-demo'
        DEPLOY_ENV = 'staging'
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Checking out branch ${env.BRANCH_NAME}"
                checkout scm
            }
        }

        stage('Parallel Build & Test') {
            parallel {
                stage('Build') {
                    steps {
                        echo "Building branch ${env.BRANCH_NAME}"
                        sh "echo Compiling code..."
                        sh "hostname"
                    }
                }

                stage('Test') {
                    steps {
                        echo "Running tests for branch ${env.BRANCH_NAME}"
                        sh "echo Tests passed!"
                    }
                }
            }
        }

        stage('Deploy') {
            when {
                expression { return env.BRANCH_NAME == 'main' } // Only deploy main
            }
            steps {
                echo "Deploying ${APP_NAME} to ${DEPLOY_ENV}"
                sh "echo Deployment complete!"
            }
        }
    }

    post {
        success {
            echo "Pipeline for ${env.BRANCH_NAME} completed successfully!"
        }
        failure {
            echo "Pipeline for ${env.BRANCH_NAME} failed. Check logs!"
        }
        always {
            echo "Cleaning workspace..."
            cleanWs()
        }
    }
}