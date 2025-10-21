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
