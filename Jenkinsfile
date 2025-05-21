pipeline {
    agent any

    environment {
        SONARQUBE = 'SonarQube'  // Tên server SonarQube mà bạn đã cấu hình trong Jenkins
        SONARQUBE_TOKEN = credentials('sqp_9de03a68db351a056e5ca74d5689c229468214ef')  // Chứa token từ Jenkins Credentials
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'dev', url: 'https://github.com/dsk-huydhd/DemoJava.git'
            }
        }

        stage('Build & Test') {
            steps {
                sh 'chmod +x gradlew'
                sh './gradlew clean test'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    // Chạy SonarQube Scanner để phân tích mã nguồn
                    withSonarQubeEnv(SONARQUBE) {
                        sh 'mvn clean install sonar:sonar -Dsonar.login=$SONARQUBE_TOKEN'
                    }
                }
            }
        }

        stage('Publish Test Report') {
            steps {
                sh 'ls -la app/build/test-results/test'
                junit 'app/build/test-results/test/*.xml'
            }
        }
    }

    post {
        success {
            echo '✅ Build and test succeeded!'
        }
        failure {
            echo '❌ Build or test failed.'
        }
    }
}
