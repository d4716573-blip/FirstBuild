pipeline {
    agent any

    environment {
        APP_NAME = "simple-java-app"
        IMAGE_NAME = "simple-java-app"
        IMAGE_TAG = "${BUILD_NUMBER}"
        CONTAINER_PORT = "8080"
        HOST_PORT = "8080"
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Maven Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .'
                sh 'docker tag ${IMAGE_NAME}:${IMAGE_TAG} ${IMAGE_NAME}:latest'
            }
        }

        stage('Stop Old Container') {
            steps {
                sh '''
                docker stop ${APP_NAME} || true
                docker rm ${APP_NAME} || true
                '''
            }
        }

        stage('Run New Container') {
            steps {
                sh '''
                docker run -d \
                  --name ${APP_NAME} \
                  -p ${HOST_PORT}:${CONTAINER_PORT} \
                  ${IMAGE_NAME}:latest
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh '''
                sleep 8
                docker ps
                curl -f http://localhost:${HOST_PORT} || exit 1
                '''
            }
        }
    }

    post {
        success {
            echo "Deployment successful. Open http://54.206.43.252:8080"
        }

        failure {
            echo "Deployment failed. Check Jenkins console logs."
            sh 'docker logs ${APP_NAME} || true'
        }
    }
}
