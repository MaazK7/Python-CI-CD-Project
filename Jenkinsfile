pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'streamlit-app'
        DOCKER_CONTAINER_NAME = 'streamlit-container'
        DOCKER_PORT = '8501'
    }
    stages {
        stage('Clone Repository') {
            steps {
                // Clone the repository
                git 'https://github.com/MaazK7/Python-CI-CD-Project.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    docker.build(DOCKER_IMAGE)
                }
            }
        }
        stage('Stop Existing Container') {
            steps {
                script {
                    // Stop and remove the existing container if it's running
                    sh '''
                    if [ $(docker ps -q -f name=${DOCKER_CONTAINER_NAME}) ]; then
                        docker stop ${DOCKER_CONTAINER_NAME}
                        docker rm ${DOCKER_CONTAINER_NAME}
                    fi
                    '''
                }
            }
        }
        stage('Deploy New Container') {
            steps {
                script {
                    // Run the new container
                    docker.image(DOCKER_IMAGE).run("-d --name ${DOCKER_CONTAINER_NAME} -p ${DOCKER_PORT}:8501")
                }
            }
        }
    }
    post {
        always {
            // Clean up any stopped containers to avoid clutter
            script {
                sh '''
                if [ $(docker ps -q -f name=${DOCKER_CONTAINER_NAME}) ]; then
                    docker stop ${DOCKER_CONTAINER_NAME}
                    docker rm ${DOCKER_CONTAINER_NAME}
                fi
                '''
            }
        }
    }
}
