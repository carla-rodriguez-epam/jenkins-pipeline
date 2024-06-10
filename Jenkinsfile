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
                        withEnv(['PORT=3000'])
                        sh 'cp /src/logo.svg ./'
                        docker.image('node').run('-d -p 3000:3000')
                    } else if (env.BRANCH_NAME == 'dev')
                        withEnv(['PORT=3001']) {
                        sh 'cp /path/to/new/logo.svg ./'
                        docker.image('node').run('-d -p 3001:3001')
                    }
                }
            }
        }
    }
}