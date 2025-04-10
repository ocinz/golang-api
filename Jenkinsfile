pipeline {
    agent any

    environment {
        // DOCKER_ENV akan berisi path file jika credential bertipe Secret file
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
                    // Salin file env dari temporary path ke workspace sebagai .env
                    sh "cp ${DOCKER_ENV} ./"
                    echo ".env file copied from Jenkins credential"
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
