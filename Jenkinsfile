pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from Git
                git 'https://github.com/carla-rodriguez-epam/jenkins-pipeline.git'
            }
        }

        stage('Build') {
            steps {
                // Build application
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                // Run tests
                sh 'npm test'
            }
        }

        stage('Build Docker Image') {
            steps {
                // Build Docker image
                sh 'docker build -t node:v1 .'
            }
        }

        stage('Deploy') {
            steps {
                // Deploy application
                script {
                    if (env.BRANCH_NAME == 'main') {
                        sh 'sed -i "s/port: 3001/port: 3000/" config.js'
                        sh 'cp /path/to/new/logo.svg ./'
                    } else if (env.BRANCH_NAME == 'dev') {
                        sh 'sed -i "s/port: 3000/port: 3001/" config.js'
                        sh 'cp /path/to/new/logo.svg ./'
                    }
                }
            }
        }
    }
}
