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

        stage('Setup .env File') {
            steps {
                script {
                    writeFile file: '.env', text: DOCKER_ENV
                    echo ".env file created!"
                }
                sh 'cat .env'
            }
        }

        stage('Docker Compose Build') {
            steps {
                sh 'docker compose --env-file .env build'
            }
        }

        stage('Docker Compose Up') {
            steps {
                sh 'docker compose --env-file .env up -d'
            }
        }
    }

    post {
        always {
            // Bersihkan .env file setelah eksekusi
            sh 'rm -f .env'
        }
        failure {
            echo '❌ Deployment failed.'
        }
        success {
            echo '✅ Deployment successful!'
        }
    }
}
