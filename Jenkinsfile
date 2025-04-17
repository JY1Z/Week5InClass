pipeline {
    agent any

    environment {
        SONARQUBE_SERVER = 'SonarQubeServer'
        SONAR_TOKEN =  'squ_297cb5472bb9beefad83b52f0488c4e9b2c84069'
        DOCKERHUB_CREDENTIALS_ID = 'Docker_Hub'
        DOCKERHUB_REPO = 'jyzh770/week5inclass'
        DOCKER_IMAGE_TAG = 'latest_v1'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/JY1Z/Week5InClass.git'
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean install'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv("${SONARQUBE_SERVER}") {
                    bat """
                        sonar-scanner ^
                        -Dsonar.projectKey=devops-demo ^
                        -Dsonar.sources=src ^
                        -Dsonar.projectName=DevOps-Demo ^
                        -Dsonar.host.url=http://localhost:9000 ^
                        -Dsonar.login=${SONAR_TOKEN} ^
                        -Dsonar.java.binaries=target/classes
                    """
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${DOCKERHUB_REPO}:${DOCKER_IMAGE_TAG}")
                }
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKERHUB_CREDENTIALS_ID) {
                        dockerImage.push()
                    }
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}

// pipeline {
//     agent any
//
//     environment {
//         SONARQUBE_SERVER = 'SonarQubeServer'  // The name of the SonarQube server configured in Jenkins
//         SONAR_TOKEN = 'squ_297cb5472bb9beefad83b52f0488c4e9b2c84069' // Store the token securely
//         // Define Docker Hub credentials ID
//         DOCKERHUB_CREDENTIALS_ID = 'Docker_Hub'
//         // Define Docker Hub repository name
//         DOCKERHUB_REPO = 'jyzh770/week5inclass'
//         // Define Docker image tag
//         DOCKER_IMAGE_TAG = 'latest_v1'
//     }
//
//     stages {
//         stage('Checkout') {
//             steps {
//                 git branch: 'main', url: 'https://github.com/JY1Z/Week5InClass.git'
//             }
//         }
//
//         stage('Build') {
//             steps {
//                 bat 'mvn clean install'
//             }
//         }
//
//         stage('SonarQube Analysis') {
//             steps {
//                 withSonarQubeEnv('SonarQubeServer') {
//                     bat """
//                         sonar-scanner ^
//                         -Dsonar.projectKey=devops-demo ^
//                         -Dsonar.sources=src ^
//                         -Dsonar.projectName=DevOps-Demo ^
//                         -Dsonar.host.url=http://localhost:9000 ^
//                         -Dsonar.login=${env.SONAR_TOKEN} ^
//                         -Dsonar.java.binaries=target/classes
//                     """
//                 }
//             }
//         }
//
//     }
//
//              stage('Build Docker Image') {
//                         steps {
//                             // Build Docker image
//                             script {
//     //                             docker.build("${DOCKERHUB_REPO}:${DOCKER_IMAGE_TAG}")
//                                 def myImage = docker.build("${DOCKERHUB_REPO}:${DOCKER_IMAGE_TAG}")
//                             }
//                         }
//                     }
//                     stage('Push Docker Image to Docker Hub') {
//                         steps {
//                             // Push Docker image to Docker Hub
//                             script {
//                                 docker.withRegistry('https://index.docker.io/v1/', DOCKERHUB_CREDENTIALS_ID) {
//     //                                 docker.image("${DOCKERHUB_REPO}:${DOCKER_IMAGE_TAG}").push()
//                                     def myImage = docker.image("${DOCKERHUB_REPO}:${DOCKER_IMAGE_TAG}")
//                                     myImage.push()
//                                 }
//                             }
//                         }
//                     }
//       }
//
