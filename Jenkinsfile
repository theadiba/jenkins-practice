pipeline {
    agent any

    environment {
        IMAGE     = 'myapp'
        CONTAINER = 'myapp-container'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t ${IMAGE}:${BUILD_NUMBER} -t ${IMAGE}:latest .'
            }
        }

        stage('Docker Run') {
            steps {
                sh '''
                    docker rm -f ${CONTAINER} || true
                    docker run -d --name ${CONTAINER} -p 5000:5000 ${IMAGE}:latest
                '''
            }
        }

        stage('Verify') {
            steps {
                sh 'sleep 3 && docker ps --filter name=${CONTAINER}'
            }
        }
    }

    post {
        success { echo "Deployed build #${env.BUILD_NUMBER}" }
        failure { echo 'Build failed, check the console output' }
    }
}
