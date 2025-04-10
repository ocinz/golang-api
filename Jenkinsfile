pipeline {
    agent any
    environment {
        DOCKER_ENV = credentials('golang-api-env')
    }
    stages {
        stage('Checkout Source') {
            steps {
                checkout scm
            }
        }
        stage(name: 'Check Environment') {
            steps {
                sh(script: 'cat ${DOCKER_ENV}')
            }
        }
        stage('Setup .env File') {
            steps {
                script {
                     writeFile file: '.env', text: DOCKER_ENV
                    echo ".env file created!"
                }
                sh  'cat .env'
            }
        }
        stage('Docker Compose Build') {
            steps {
                sh 'docker compose build'
            }
        }
        stage('Docker Compose Up') {
            steps {
                sh 'docker compose up -d'
            }
        }
    }
    post {
        failure {
            echo '❌ Deployment failed.'
        }
        success {
            echo '✅ Deployment successful!'
        }
    }
}
