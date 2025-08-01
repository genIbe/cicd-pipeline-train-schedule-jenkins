pipeline {
    agent any

    tools {
        // Use a compatible JDK (Java 17)
        jdk 'java-17'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                // Run npm install through Gradle's npmInstall task
                sh './gradlew npmInstall'
            }
        }

        stage('Test') {
            steps {
                // Run npm tests through Gradle
                sh './gradlew npmTest'
            }
        }

        stage('Build App') {
            steps {
                // Runs npmBuild -> zipApp -> jar build
                sh './gradlew clean build'
            }
        }

        stage('Archive Artifacts') {
            steps {
                // Archive JARs and zipped dist directory
                archiveArtifacts artifacts: 'build/libs/*.jar', fingerprint: true
                archiveArtifacts artifacts: 'dist/*.zip', fingerprint: true
            }
        }
    }

    post {
        always {
            echo 'Cleaning workspace...'
            cleanWs()
        }
        success {
            echo '✅ Build completed successfully!'
        }
        failure {
            echo '❌ Build failed. Check console output for details.'
        }
    }
}

