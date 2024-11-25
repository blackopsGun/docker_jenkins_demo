pipeline {
    agent any
    
    environment {
        GIT_REPOSITORY_URL = 'https://github.com/blackopsGun/docker_jenkins_demo.git'
        DOCKER_IMAGE_NAME = 'blackopsgun/16-docker_jenkins_demo'
        IMAGE_TAG = '1.0'
    }
    
    stages {
        stage('Clone Repository') {
            steps {
                script {
                    try {
                        git branch: 'main', url: GIT_REPOSITORY_URL
                    } catch (Exception e) {
                        echo "Failed to clone repo: ${e.message}"
                        error "Failed to clone repository"
                    }
                }
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    try {
                        docker.build("${DOCKER_IMAGE_NAME}:${IMAGE_TAG}")
                    } catch (Exception e) {
                        echo "Failed to build Docker image: ${e.message}"
                        error "Docker image build failed"
                    }
                }
            }
        }
        
        stage('Push to Docker Hub') {
            steps {
                script {
                    try {
                        withCredentials([usernamePassword(credentialsId: '1234', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                            sh """
                                docker push ${DOCKER_IMAGE_NAME}:${IMAGE_TAG}
                            """
                        }
                    } catch (Exception e) {
                        echo "Failed to push image to Docker Hub: ${e.message}"
                        error "Docker push failed"
                    }
                }
            }
        }
    }
}
