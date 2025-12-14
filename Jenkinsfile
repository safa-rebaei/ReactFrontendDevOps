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
                sh 'npm install'
            }
        }
        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }
        stage('Run Docker') {
            steps {
                sh 'docker build -t react-frontend .'
                sh 'docker run -d -p 3000:80 react-frontend'
            }
        }
    }
}
