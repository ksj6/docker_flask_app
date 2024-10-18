pipeline {
    agent any

    environment {
        IMAGE_NAME = "docker_assignment3"
        DOCKER_REGISTRY = "docker.io/ksj66"  // Optional: for pushing images to a registry
        
        DOCKER_CREDENTIALS = credentials('dockerhub-credentials')  // Using the ID you defined

    }

    stages {
        stage('Clone Repository') {
            steps {
                // Pull the source code from version control
                git url: 'https://github.com/ksj6/docker_flask_app.gitt', branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    sh 'docker build -t $IMAGE_NAME .'
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    // Run the application container and test it
                    sh '''
                    docker run -d --name test-container -p 5000:5000 $IMAGE_NAME
                    sleep 5  # Wait for the app to start
                    curl -f http://localhost:5000 || exit 1  # Check if the app is running
                    docker stop test-container
                    docker rm test-container
                    '''
                }
            }
        }

        stage('Push to Docker Registry') {
            when {
                expression {
                    return env.DOCKER_REGISTRY != null && env.DOCKER_REGISTRY != ""
                }
            }
            steps {
                script {
                    // Push the Docker image to a Docker registry
                    sh '''
                    docker tag $IMAGE_NAME $DOCKER_REGISTRY/$IMAGE_NAME
                    docker push $DOCKER_REGISTRY/$IMAGE_NAME
                    '''
                }
            }
        }
    }

    post {
        always {
            // Clean up Docker images after the build process
            script {
                sh 'docker rmi $IMAGE_NAME || true'
            }
        }
        failure {
            // Notify about build failures
            echo 'Build failed!'
        }
        success {
            echo 'Build and tests were successful!'
        }
    }
}