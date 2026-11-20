pipeline {
    agent any

    environment {
        VM_IP        = 'localhost'
        BACKEND_IMG  = 'azizhmani20/lms-backend:1.0.0'
        FRONTEND_IMG = 'azizhmani20/lms-frontend:1.0.0'
        COMPOSE_PROJECT_NAME = 'lms'
    }

    stages {
        stage('Git') {
            steps {
                checkout scm
            }
        }

        stage('Building image') {
            steps {
                sh 'docker build -t $BACKEND_IMG ./lms-backend'
                sh """
                   sed -i 's|http://localhost:8080|http://${VM_IP}:8082|g' lms-frontend/src/app/_service/*.ts
                   docker build -t $FRONTEND_IMG ./lms-frontend
                """
            }
        }

        stage('Deploy Image') {
            steps {
                withCredentials([usernamePassword(
                        credentialsId: 'dockerhub-creds',
                        usernameVariable: 'HUB_USER',
                        passwordVariable: 'HUB_PASS')]) {
                    sh 'echo "$HUB_PASS" | docker login -u "$HUB_USER" --password-stdin'
                    sh 'docker push $BACKEND_IMG'
                    sh 'docker push $FRONTEND_IMG'
                }
            }
        }

        stage('Docker Compose') {
            steps {
                sh 'docker compose down'
                sh 'docker compose up -d'
                sh 'docker compose ps'
            }
        }
    }

    post {
        always {
            sh 'docker logout || true'
        }
    }
}
