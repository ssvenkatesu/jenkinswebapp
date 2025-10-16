pipeline {
    agent any

    environment {
        IMAGE_NAME = "simple-webapp"
        CONTAINER_NAME = "webapp-container"
        PORT = "80"
    }

    stages {

        stage('Checkout Code') {
            steps {
                echo "Cloning repository..."
                git branch: 'main', url: 'https://github.com/ssvenkatesu/jenkinswebapp'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image..."
                script {
                    // Build Docker image
                    sh "docker build -t ${IMAGE_NAME}:latest ."
                }
            }
        }

        stage('Deploy Container') {
            steps {
                echo "Deploying container..."
                script {
                    // Stop and remove old container if it exists
                    sh "docker ps -q --filter name=${CONTAINER_NAME} | grep -q . && docker rm -f ${CONTAINER_NAME} || echo 'No existing container to remove'"
                    
                    // Run new container
                    sh "docker run -d --name ${CONTAINER_NAME} -p ${PORT}:80 ${IMAGE_NAME}:latest"
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check logs for errors.'
        }
    }
}
