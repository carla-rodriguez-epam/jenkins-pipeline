// DEV BRANCH
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
                    if (env.BRANCH_NAME == 'main') {
                        docker.build('nodemain:v1.0')
                    } else if (env.BRANCH_NAME == 'dev') {
                        docker.build('nodedev:v1.0')
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'main') {
                        docker.image('nodemain:v1.0').run('-d -p 3000:3000')
                    } else if (env.BRANCH_NAME == 'dev') {
                        docker.image('nodedev:v1.0').run('-d -p 3001:3000')
                    }
                }
            }
        }
    }
}