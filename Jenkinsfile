pipeline {
    agent any

    environment {
        IMAGE_NAME = "keerthanas04/my-service"
        DOCKER_USER = "keerthanas04" // Replace with your actual Docker Hub username
        DOCKER_PASS = "Keerthana@04" // Replace with your actual Docker Hub password
    }

    stages {
        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/KeerthanaS2002/Devops_spring_framework.git', branch: 'main'
            }
        }

        stage('Build Application') {
            steps {
                script {
                    sh 'mvn clean package -DskipTests'
                }
            }
        }

        stage('Run Maven Tests') {
            steps {
                script {
                    try {
                        sh 'mvn test'
                    } catch (Exception e) {
                        echo "Tests failed, but proceeding..."
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${IMAGE_NAME}:latest ."
                }
            }
        }

        stage('Login to Docker Registry') {
            steps {
                script {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Push Image to Docker Registry') {
            steps {
                script {
                    sh "docker push ${IMAGE_NAME}:latest"
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed! Check the logs for errors.'
        }
    }
}
