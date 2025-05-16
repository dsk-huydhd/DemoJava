pipeline {
    agent any

    // tools {
    //     jdk 'jdk-21' // Đặt tên theo Global Tool Configuration
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

        stage('Publish Test Report') {
            steps {
                sh 'ls -la app/build/test-results/test'
                junit 'app/build/test-results/test/*.xml'
            }
        }
    }
    // post {
    //     always {
    //         archiveArtifacts artifacts: 'build/libs/*.jar'
    //     }
    // }
        post {
        success {
            echo '✅ Build and test succeeded!'
        }
        failure {
            echo '❌ Build or test failed.'
        }
    }
}