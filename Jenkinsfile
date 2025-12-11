pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'SEU_REPOSITORIO_GIT'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t astro-app:latest .'
            }
        }

        stage('Stop Old Container') {
            steps {
                sh 'docker stop astro-app || true'
                sh 'docker rm astro-app || true'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker run -d --name astro-app -p 8085:80 astro-app:latest'
            }
        }
    }
}
