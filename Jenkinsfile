pipeline {
    agent any
    
    environment {
        APP_NAME = 'web'
        REPO_URL ="https://github.com/mmohamedELsayed-ai/Jenkinsfile.git"
    }

    stages {
        stage('Getting Repo files') {
            steps {
                git branch: "${GIT_BRANCH}", credentialsId: 'github', url: "${REPO_URL}"
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image
                    sh 'docker build -t ${APP_NAME}:${BUILD_NUMBER} .'
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Run Docker image with build number in container name
                    sh """
                        docker run --name ${APP_NAME}-${BRANCH_NAME}-${BUILD_NUMBER} -p 5000:80 -d ${APP_NAME}:${BUILD_NUMBER}
                        docker ps
                    """
                }
            }
        }
    }
}
