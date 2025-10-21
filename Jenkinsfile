pipeline {
    agent any
    tools {
        jdk 'jdk17'          // ✅ Jenkins JDK name you added
        maven 'Maven 3'      // your Maven installation name
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'git@github.com:kumarsushmitha33/customer-service.git'
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
                    def imageName = "sushmithakumar1512/customer-service"
                    sh "docker build -t ${imageName}:latest ."
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                        sh "docker push ${imageName}:latest"
                    }
                }
            }
        }
    }
    post {
        success {
            echo '✅ Customer Service build & test successful!'
        }
        failure {
            echo '❌ Build failed!'
        }
    }
}
