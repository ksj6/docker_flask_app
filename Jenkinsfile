pipeline {
    agent any

    environment {
        GIT_CREDENTIALS = credentials('git-credentials') // Ensure you have this credential ID set in Jenkins
    }

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    // Clone the repository using the provided credentials
                    sh "git clone https://${GIT_CREDENTIALS_USR}:${GIT_CREDENTIALS_PSW}@your-repo-url.git"
                    
                    // Change directory to the cloned repository
                    dir('docker_flask_app') {
                        // You may need to replace 'your-repo-folder-name' with the actual folder name after cloning
                        echo "Repository cloned successfully."
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Navigate to the cloned repository directory and build the Docker image
                    dir('docker_flask_app') {
                        sh 'docker build -t hello-world-app .'
                    }
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Navigate to the cloned repository directory and run the Docker container
                    dir('docker_flask_app') {
                        sh 'docker run -d -p 5000:5000 hello-world-app'
                    }
                }
            }
        }
    }

    post {
        always {
            script {
                // Clean up any running Docker containers
                sh 'docker ps -a -q --filter name=hello-world-app | xargs --no-run-if-empty docker stop'
                sh 'docker ps -a -q --filter name=hello-world-app | xargs --no-run-if-empty docker rm'
            }
        }
    }
}
