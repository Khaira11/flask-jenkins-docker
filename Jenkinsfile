pipeline {
    agent any

    environment {
        // Replace with your actual DockerHub repo: username/repo
        IMAGE_NAME = "khaira23/flask-jenkins"
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    echo "🔨 Building Docker image: ${IMAGE_NAME}"
                    docker.build("${IMAGE_NAME}")
                }
            }
        }

        stage('Login & Push to DockerHub') {
            steps {
                script {
                    echo "🔐 Logging in and pushing to DockerHub"
                    docker.withRegistry('https://index.docker.io/v1/', 'Docker_hub') {
                        docker.image("${IMAGE_NAME}").push('latest')
                    }
                }
            }
        }
    }

    post {
        success {
            echo "✅ Build and push successful!"
        }
        failure {
            echo "❌ Build or push failed. Check the logs."
        }
    }
}

