pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "app1"
        DOCKER_REGISTRY = "narula2712"
        IMAGE_TAG = "${DOCKER_REGISTRY}/${DOCKER_IMAGE}:${BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Prakriti271292/samplerepo.git'
            }
        }

        stage('Setup') {
            steps {
                sh '''
                    python3 -m venv .venv
                    . .venv/bin/activate
                    pip install --upgrade pip
                    pip install flask pytest
                '''
            }
        }

        stage('Test') {
            steps {
                sh '''
                    . .venv/bin/activate
                    pytest
                '''
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'dockerhubpwd', variable: 'dockerhubpwd')]) {
                    sh """
                        docker login -u narula2712 -p ${dockerhubpwd}
                    """
                }
                echo 'Login successfully'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh """
                    sudo docker build -t ${IMAGE_TAG} .
                """
                echo "Docker image built successfully"
                sh "docker images"
            }
        }

        stage('Push Docker Image') {
            steps {
                sh "docker push ${IMAGE_TAG}"
                echo "Docker image pushed successfully"
            }
        }
    }
}
