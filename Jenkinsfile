pipeline {
    agent any
    tools {
        jdk 'jdk17'
        maven 'Maven 3'
    }
    stages {
        /*stage('Checkout') {
            steps {
                git branch: 'main', url: 'git@github.com:kumarsushmitha33/customer-service.git'
            }
        }*/ 
        /* For webhooks- the github URL should be like below. */
        stage('Checkout') {
            steps {
                echo '📥 Checking out from GitHub using PAT...'
                git branch: 'main',
                    credentialsId: 'github-creds',
                    url: 'https://github.com/kumarsushmitha33/customer-service.git'
            }
        }
        stage('Build') {
            steps {
                echo '🔧 Building Customer Service...'
                sh 'mvn clean install -DskipTests'
            }
        }
        stage('Test') {
            steps {
                echo '🧪 Running Tests...'
                sh 'mvn test'
            }
        }
        stage('Docker Build & Push') {
            steps {
                echo '🐳 Building and pushing Docker image...'
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh '''
                        /usr/local/bin/docker build -t $DOCKER_USER/customer-service:latest .
                        echo $DOCKER_PASS | /usr/local/bin/docker login -u $DOCKER_USER --password-stdin
                        /usr/local/bin/docker push $DOCKER_USER/customer-service:latest
                        '''
                    }
                }
            }
        }
       stage('Deploy') {
    steps {
        echo '🚀 Deploying Customer Service container...'
        script {
            withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                sh '''
                # Stop and remove any old container
                /usr/local/bin/docker stop customer-service || true
                /usr/local/bin/docker rm customer-service || true

                # Pull latest image from Docker Hub
                /usr/local/bin/docker pull $DOCKER_USER/customer-service:latest

                # Run new container
                /usr/local/bin/docker run -d -p 8085:8080 --name customer-service $DOCKER_USER/customer-service:latest
                '''
            }
        }
    }
}
    }
    post {
        success {
            echo '✅ Customer Service build, push & deploy successful!'
        }
        failure {
            echo '❌ Build failed!'
        }
    }
}