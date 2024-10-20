pipeline {
    agent any

    environment {
        GIT_REPO_URL = 'https://github.com/ksj6/docker_flask_app.git' // Include the protocol
    }

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    // Use withCredentials to handle sensitive information securely
                    withCredentials([usernamePassword(credentialsId: 'git-credentials', usernameVariable: 'GIT_USER', passwordVariable: 'GIT_PASS')]) {
                        // Configure Git to store credentials temporarily
                        sh '''
                            git config --global credential.helper store
                            echo "${GIT_USER}:${GIT_PASS}" > ~/.git-credentials
                            git clone ${GIT_REPO_URL}
                            rm ~/.git-credentials
                        '''
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Navigate to the cloned repository directory and build the Docker image
                    dir('docker_flask_app') {
                        sh 'docker build -t docker_flask_app .'
                    }
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Run the Docker container
                    dir('docker_flask_app') {
                        sh 'docker run -d -p 5000:5000 docker_flask_app'
                    }
                }
            }
        }
    }

    post {
        always {
            script {
                // Clean up any running Docker containers
                sh 'docker ps -a -q --filter name=docker_flask_app | xargs --no-run-if-empty docker stop'
                sh 'docker ps -a -q --filter name=docker_flask_app | xargs --no-run-if-empty docker rm'
            }
        }
    }
}
