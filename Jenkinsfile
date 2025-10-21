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
        echo "🐳 Building and pushing Docker image..."
        script {
            // Use absolute path for Docker binary
            sh '/usr/local/bin/docker build -t sushmithakumar1512/customer-service:latest .'
            sh 'echo $DOCKER_PASSWORD | /usr/local/bin/docker login -u sushmithakumar1512 --password-stdin'
            sh '/usr/local/bin/docker push sushmithakumar1512/customer-service:latest'
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
