pipeline {
    agent any

    environment {
        NAMESPACE = "default"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/your-repo/nginx-php-mysql.git'
            }
        }

        stage('Build Docker Images') {
            steps {
                script {
                    sh 'docker build -t my-nginx ./nginx'
                    sh 'docker build -t my-php74 ./php74'
                    sh 'docker build -t my-php80 ./php80'
                    sh 'docker build -t my-mysql ./mysql'
                }
            }
        }

        stage('Push Docker Images') {
            steps {
                script {
                    sh 'docker tag my-nginx registry.example.com/my-nginx:latest'
                    sh 'docker tag my-php74 registry.example.com/my-php74:latest'
                    sh 'docker tag my-php80 registry.example.com/my-php80:latest'
                    sh 'docker tag my-mysql registry.example.com/my-mysql:latest'
                    
                    sh 'docker push registry.example.com/my-nginx:latest'
                    sh 'docker push registry.example.com/my-php74:latest'
                    sh 'docker push registry.example.com/my-php80:latest'
                    sh 'docker push registry.example.com/my-mysql:latest'
                }
            }
        }

        stage('Deploy to K8s') {
            steps {
                script {
                    sh 'kubectl apply -f k8s/mysql-deployment.yaml -n $NAMESPACE'
                    sh 'kubectl apply -f k8s/php74-deployment.yaml -n $NAMESPACE'
                    sh 'kubectl apply -f k8s/php80-deployment.yaml -n $NAMESPACE'
                    sh 'kubectl apply -f k8s/nginx-deployment.yaml -n $NAMESPACE'
                }
            }
        }
    }
}
