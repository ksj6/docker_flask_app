// pipeline {
//     agent any

//     stages {
//         stage('Clone Repository') {
//             steps {
//                 // Clone the repository using the Git URL configured in the Jenkins job
//                 git credentialsId: 'git-credentials', url: 'https://github.com/ksj6/docker_flask_app.git' // Use your actual repo URL
//             }
//         }

//         stage('Build Docker Image') {
//             steps {
//                 script {
//                     // Build the Docker image using the Dockerfile in the cloned repository
//                     bat 'docker build -t docker_flask_app docker_flask_app\\'
//                 }
//             }
//         }

//         stage('Run Docker Container') {
//             steps {
//                 script {
//                     // Run the Docker container, mapping port 5000
//                     bat 'docker run -d -p 5000:5000 docker_flask_app'
//                 }
//             }
//         }
//     }

//     post {
//         always {
//             script {
//                 // Clean up any running Docker containers
//                 bat 'docker ps -a -q --filter name=docker_flask_app | xargs --no-run-if-empty docker stop'
//                 bat 'docker ps -a -q --filter name=docker_flask_app | xargs --no-run-if-empty docker rm'
//             }
//         }
//     }
// }
