pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from Git
                git branch: "${env.BRANCH_NAME}", url: 'https://github.com/carla-rodriguez-epam/jenkins-pipeline.git'
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
                sh 'docker build -t node .'
            }
        }

        stage('Deploy') {
            steps {
                // Deploy application
                script {
                    if (env.BRANCH_NAME == 'main') {
                        sh 'sed -i "s/port: 3001/port: 3000/" config.js'
                        sh 'cp /src/logo.svc ./'
                        sh 'docker run -d -p 3000:3000 node:v1'
                    } else if (env.BRANCH_NAME == 'dev') {
                        sh 'sed -i "s/port: 3000/port: 3001/" config.js'
                        sh 'cp /path/to/new/logo.svg ./'
                        sh 'docker run -d -p 3001:3001 node:v1'
                    }
                }
            }
        }
    }
}
