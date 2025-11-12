pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "sentiment_app"
        CONTAINER_NAME = "sentiment_container"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Adarshthakur-850/sentiment_deploy.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${DOCKER_IMAGE} .'
            }
        }

        stage('Stop Old Container') {
            steps {
                sh '''
                if [ $(docker ps -q -f name=${CONTAINER_NAME}) ]; then
                    docker stop ${CONTAINER_NAME}
                    docker rm ${CONTAINER_NAME}
                fi
                '''
            }
        }

        stage('Run New Container') {
            steps {
                sh 'docker run -d -p 5000:5000 --name ${CONTAINER_NAME} ${DOCKER_IMAGE}'
            }
        }

        stage('Verify Deployment') {
            steps {
                sh 'curl -f http://localhost:5000 || exit 1'
            }
        }
    }

    post {
        success {
            echo "✅ Sentiment App Deployed Successfully!"
        }
        failure {
            echo "❌ Deployment Failed!"
        }
    }
}
