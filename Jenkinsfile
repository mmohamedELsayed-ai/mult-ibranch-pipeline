pipeline {
    agent any

    environment {
        APP_NAME = 'web-app'
        REPO_URL = "https://github.com/MohamedMagdy840/jenkins-repo.git"
        DOCKERHUB_USER = 'mohamed'   // غيّرها باسمك
        IMAGE_NAME = "${DOCKERHUB_USER}/${APP_NAME}"
    }

    stages {

        stage('Getting Repo files') {
            steps {
                git branch: "${GIT_BRANCH}", credentialsId: 'github', url: "${REPO_URL}"
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} ."
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh """
                        docker login -u $DOCKER_USER -p $DOCKER_PASS
                        docker push ${IMAGE_NAME}:${BUILD_NUMBER}
                    """
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                sh """
                    docker run -d --name ${APP_NAME}-${GIT_BRANCH}-${BUILD_NUMBER} ${IMAGE_NAME}:${BUILD_NUMBER}
                    docker ps
                """
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline SUCCESS: Docker image built and pushed successfully"
        }
        failure {
            echo "❌ Pipeline FAILED: Please check the logs"
        }
        always {
            echo "🔚 Pipeline Finished"
        }
    }
}
