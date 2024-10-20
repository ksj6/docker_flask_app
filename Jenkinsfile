pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image using the Dockerfile
                    sh 'docker build -t hello-world-app .'
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Run the Docker container
                    sh 'docker run -d -p 5000:5000 hello-world-app'
                }
            }
        }
    }

    post {
        always {
            // Clean up the Docker container after the build
            sh 'docker ps -a -q --filter name=hello-world-app | xargs --no-run-if-empty docker stop'
            sh 'docker ps -a -q --filter name=hello-world-app | xargs --no-run-if-empty docker rm'
        }
    }
}
