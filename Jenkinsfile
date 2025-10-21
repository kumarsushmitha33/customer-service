pipeline {
    agent any
    tools {
        maven 'Maven 3' // make sure Maven is configured in Jenkins
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
