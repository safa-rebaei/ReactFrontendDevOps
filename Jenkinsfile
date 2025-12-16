pipeline {
    agent any

    options {
        timestamps()
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Setup') {
            steps {
                bat 'node -v'
                bat 'npm -v'
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

        stage('Docker Build') {
            when {
                anyOf {
                    branch 'dev'
                    branch 'main'
                    tag "*"
                }
            }
            steps {
                bat 'docker build -t react-frontend .'
            }
        }

        stage('Run Docker (Smoke)') {
            when {
                anyOf {
                    branch 'dev'
                    branch 'main'
                }
            }
            steps {
                bat '''
                docker rm -f react-smoke || exit 0
                docker run -d -p 3005:80 --name react-smoke react-frontend
                timeout /t 5
                '''
            }
        }

        stage('Smoke Test') {
            when {
                anyOf {
                    branch 'dev'
                    changeRequest()
                }
            }
            steps {
                bat '''
                curl http://localhost:3005 || exit 1
                echo SMOKE_TEST=PASSED > smoke.txt
                '''
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: '**/*.txt', fingerprint: true
            }
        }
    }

    post {
        always {
            bat 'docker rm -f react-smoke || exit 0'
        }
    }
}
