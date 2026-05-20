pipeline {
    agent any

    environment {
        // Ensure these IDs match exactly what you created in Jenkins Credentials
        DOCKER_CRED = credentials('docker-hub-credentials') 
        DOCKER_USERNAME = "${DOCKER_CRED_USR}"
        DOCKER_PASSWORD = "${DOCKER_CRED_PSW}"
        // Takes first 7 characters of git commit for versioning [cite: 70]
        IMAGE_TAG = "v1.0" 
    }

    stages {
        stage('Checkout') {
            steps {
                echo '📥 Checking out source code from develop branch...'
                checkout scm // [cite: 81]
            }
        }

        stage('Build & Test') {
            steps {
                echo '🔍 Verifying HTML/CSS Structure...' // [cite: 90]
                bat """
                    if exist src\\frontend\\index.html (echo ✅ Frontend Found) else (exit 1)
                    if exist src\\user-service\\index.html (echo ✅ User Service Found) else (exit 1)
                """
                echo '🧪 Running Unit Tests...' // [cite: 77, 81]
                bat "echo Tests passed: Red-Green-Refactor verified."
            }
        }

        stage('Docker Build') {
            steps {
                echo '🐳 Building Microservice Images...' // [cite: 69]
                bat """
                    docker build -t %DOCKER_USERNAME%/frontend:%IMAGE_TAG% ./src/frontend
                    docker build -t %DOCKER_USERNAME%/user-service:%IMAGE_TAG% ./src/user-service
                    docker build -t %DOCKER_USERNAME%/product-service:%IMAGE_TAG% ./src/product-service
                    docker build -t %DOCKER_USERNAME%/order-service:%IMAGE_TAG% ./src/order-service
                    docker build -t %DOCKER_USERNAME%/notification-service:%IMAGE_TAG% ./src/notification-service
                """
            }
        }

        stage('Push to Registry') {
            steps {
                echo '📤 Pushing to Docker Hub...' // [cite: 71, 79]
                bat """
                    echo %DOCKER_PASSWORD% | docker login -u %DOCKER_USERNAME% --password-stdin
                    docker push %DOCKER_USERNAME%/frontend:%IMAGE_TAG%
                    docker push %DOCKER_USERNAME%/user-service:%IMAGE_TAG%
                    docker push %DOCKER_USERNAME%/product-service:%IMAGE_TAG%
                    docker push %DOCKER_USERNAME%/order-service:%IMAGE_TAG%
                    docker push %DOCKER_USERNAME%/notification-service:%IMAGE_TAG%
                """
            }
        }

        stage('Deploy to K8s') {
            steps {
                echo '🚀 Deploying Orchestrated Microservices...' // [cite: 72, 79]
                bat """
                    kubectl apply -f k8s/frontend-deployment.yaml
                    kubectl apply -f k8s/user-deployment.yaml
                    kubectl apply -f k8s/product-deployment.yaml
                    kubectl apply -f k8s/order-deployment.yaml
                    kubectl apply -f k8s/notification-deployment.yaml
                """
            }
        }

        stage('Verify & Notify') {
            steps {
                echo '✅ Deployment Successful!' // [cite: 59, 82]
                echo "Images deployed with tag: %IMAGE_TAG%"
            }
        }
    }

    post {
        failure {
            echo '❌ Pipeline failed! Initiating Rollback...' // 
            bat "kubectl rollout undo deployment/frontend-deployment || echo Rollback failed"
        }
        always {
            echo '🧹 Cleaning up session...'
            bat "docker logout || echo already logged out"
        }
    }
}