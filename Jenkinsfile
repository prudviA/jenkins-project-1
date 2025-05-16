pipeline {
    agent any

    environment {
        IMAGE_NAME = 'springboot-thymeleaf.crud'
        CONTAINER_NAME = 'cont2'
        APP_PORT = '8082'
    }

    stages {
        stage('Build with Maven') {
            steps {
                sh 'mvn clean install -DskipTests'
            }
        }

        stage('Stop Existing container') {
            steps {
                script {
                    sh """
                    if [ \$(docker ps -q -f name=$CONTAINER_NAME) ]; then
                        docker stop $CONTAINER_NAME || true
                        docker rm $CONTAINER_NAME  || true
                    fi
                    """
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker run -d -p 8081:$APP_PORT --name $CONTAINER_NAME $IMAGE_NAME'
            }
        }
    }

    post {
        success {
            echo "Deployment successful. App running on port 8082"
        }
        failure {
            echo "Something went wrong!"
        }
    }
}

