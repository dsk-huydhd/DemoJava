pipeline {
    agent any

    // environment {
    //     SONARQUBE = 'SonarQube'  // Tên server SonarQube mà bạn đã cấu hình trong Jenkins
    //     SONARQUBE_TOKEN = credentials('sqa_d6683a07bc0a7ad1699058fa5740b716e82376fa')  // Chứa token từ Jenkins Credentials
    // }

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
                    // sh './gradlew sonar'
                    // Chạy SonarQube Scanner để phân tích mã nguồn
                    withSonarQubeEnv(SONARQUBE) {
                        sh 'mvn clean install sonar:sonar -Dsonar.login=sqa_d6683a07bc0a7ad1699058fa5740b716e82376fa'
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
