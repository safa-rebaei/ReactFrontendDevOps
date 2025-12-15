pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install') {
            steps {
                bat 'npm install'
            }
        }

        stage('Build') {
            steps {
                bat 'npm run build'
            }
        }

        stage('Docker Build & Run') {
            steps {
                bat '''
                docker rm -f react-frontend || exit 0
                docker build -t react-frontend .
                docker run -d -p 3000:80 --name react-frontend react-frontend
                '''
            }
        }
    }
}
