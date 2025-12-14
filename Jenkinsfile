pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Docker Build') {
            steps {
                // Build de l'image Docker à partir du Dockerfile
                bat 'docker build -t react-frontend .'
            }
        }

        stage('Run Docker (Smoke Test)') {
            steps {
                // Supprimer le conteneur si déjà existant et lancer un nouveau
                bat '''
                docker rm -f react-frontend-test || exit 0
                docker run -d -p 3001:80 --name react-frontend-test react-frontend
                '''
            }
        }
    }

    post {
        always {
            bat 'docker stop react-frontend-test || exit 0'
            bat 'docker rm react-frontend-test || exit 0'
        }
    }
}
