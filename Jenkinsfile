pipeline {
    agent any

    tools { 
        nodejs 'node' 
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: "${env.BRANCH_NAME}", url: 'https://github.com/carla-rodriguez-epam/jenkins-pipeline.git'
            }
        }

        stage('Build') {
            steps {
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build('node')
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'main') {
                        sh 'sed -i "s/port: 3001/port: 3000/" config.js'
                        sh 'cp /src/logo.svc ./'
                        docker.image('node').run('-d -p 3000:3000')
                    } else if (env.BRANCH_NAME == 'dev') {
                        sh 'sed -i "s/port: 3000/port: 3001/" config.js'
                        sh 'cp /path/to/new/logo.svg ./'
                        docker.image('node').run('-d -p 3001:3001')
                    }
                }
            }
        }
    }
}