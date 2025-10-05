pipeline {
    agent any

    environment {
        IMAGE_NAME = "daouda482/react-frontend"
        IMAGE_TAG = "latest"
    }

    stages {

        stage('Checkout') {
            steps {
                echo "Récupération du code source depuis GitHub..."
                git branch: 'main', url: 'https://github.com/daouda482/aws-devops.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Construction de l'image Docker..."
                sh 'docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ./front-end'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                echo "Connexion à Docker Hub..."
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                echo "Push de l'image sur Docker Hub..."
                sh 'docker push ${IMAGE_NAME}:${IMAGE_TAG}'
            }
        }

        stage('Clean Up') {
            steps {
                echo "Nettoyage des images locales..."
                sh 'docker rmi ${IMAGE_NAME}:${IMAGE_TAG} || true'
            }
        }
    }

    post {
        success {
            echo '✅ Build et push Docker terminés avec succès !'
        }
        failure {
            echo '❌ Le pipeline a échoué.'
        }
    }
}
