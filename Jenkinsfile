// pipeline {
//     agent any
//     tools {
//         nodejs 'NodeJS_14'
//     }
//     stages {
//         stage('Fetch code') {
//             steps {
//                 git branch: 'main', url: 'https://github.com/jamesngn/brewhub-app.git'
//             }
//             post {
//                 success {
//                     echo 'Fetch Code successfully'
//                 }
//             }
//         }
//         stage('Start MongoDB Server') {
//             steps {
//                 script {
//                     sh 'mongosh'
//                 }
//             }
//         }
//         stage('Start gRPC Server') {
//             steps {
//                 script {
//                     dir('./services/shared/') {
//                         sh 'npm install' // Clean and install Node.js modules
//                         sh 'npm rebuild' // Rebuild any binary modules
//                     }
//                     dir('./services/auth-service/') {
//                         sh 'npm install' // Clean and install Node.js modules
//                         sh 'npm rebuild' // Rebuild any binary modules
//                         sh 'npm start' // Start your server
//                     }
//                 }
//                 post {
//                     success {
//                         echo 'Start gRPC auth-service successfully'
//                     }
//                 }
//             }
//         }
//     }
// }

//DOCKER:
//start service - build test
pipeline {
    agent any

    stages {
        stage('Build auth-service Docker Image') {
            steps {
                script {
                    // Build Docker image for auth-service
                    dir('services/auth-service') {
                        docker.build('auth-service', '.')
                    }
                }
            }
        }
        stage('Start Services') {
            steps {
                script {
                    //Start MongoDB
                    sh 'docker run -d --name mongodb mongo:4'
                    //Start auth-service
                    sh 'docker run -d --name auth-service -p 5054:5054 auth-service'
                }
            }
        }
    }
}