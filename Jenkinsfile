pipeline {
    agent any

    tools {
        jdk 'java-17'
    }

    environment {
        GRADLE_OPTS = "-Dorg.gradle.daemon=false"
        PATH = "/usr/local/bin:$PATH" // Ensure npm is in PATH
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Node Dependencies') {
            steps {
                sh './gradlew npmInstall'
            }
        }

        stage('Run Tests') {
            steps {
                sh './gradlew npmTest'
            }
        }

        stage('Build & Package') {
            steps {
                sh './gradlew clean build'
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: 'build/libs/*.jar', fingerprint: true
                archiveArtifacts artifacts: 'dist/*.zip', fingerprint: true
            }
        }
    }

    post {
        always {
            cleanWs()
        }
        success {
            echo '✅ Build completed successfully!'
        }
        failure {
            echo '❌ Build failed. Check console logs for details.'
        }
    }
}

