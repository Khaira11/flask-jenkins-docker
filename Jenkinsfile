pipeline {
    agent any

    environment {
        // Your DockerHub image name
        IMAGE_NAME = "khaira11/flask-jenkins"
        CONTAINER_NAME = "flask-jenkins-app"
        HOST_PORT = "5000"
        CONTAINER_PORT = "5000"
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
                    docker.withRegistry('https://index.docker.io/v1/', 'docker-hub-credentials') {
                        docker.image("${IMAGE_NAME}").push('latest')
                    }
                }
            }
        }

        stage('Deploy Container') {
            steps {
                script {
                    echo "🚀 Deploying container from DockerHub"

                    // Stop and remove any running container with the same name
                    sh """
                        if [ \$(docker ps -aq -f name=\$CONTAINER_NAME) ]; then
                            echo "⚠️ Removing existing container: \$CONTAINER_NAME"
                            docker stop \$CONTAINER_NAME || true
                            docker rm \$CONTAINER_NAME || true
                        fi
                    """

                    // Pull the latest image
                    sh "docker pull ${IMAGE_NAME}:latest"

                    // Run the container on port 5000
                    sh """
                        docker run -d --name \$CONTAINER_NAME -p \$HOST_PORT:\$CONTAINER_PORT ${IMAGE_NAME}:latest
                    """

                    echo "✅ Container deployed and accessible at http://localhost:${HOST_PORT}"
                }
            }
        }
    }

    post {
        success {
            echo "🎉 Pipeline completed successfully: Build, Push, Deploy"
        }
        failure {
            echo "❌ Something failed. Check the build logs."
        }
    }
}

