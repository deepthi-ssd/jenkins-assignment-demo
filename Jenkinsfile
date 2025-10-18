pipeline {
    agent {
        docker {
            image 'node:18-alpine' // Node.js Docker image
            args '-u root:root'    // Run as root to avoid permission issues
        }
    }

    environment {
        EMAIL_RECIPIENT = 'raimund@rittnauer.at'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/deepthi-ssd/jenkins-assignment-demo.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test'
            }
        }

        stage('Build Application') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: 'dist/**', allowEmptyArchive: true
            }
        }

        stage('Publish HTML Report') {
            steps {
                script {
                    if (fileExists('dist/index.html')) {
                        publishHTML(target: [
                            reportName: 'App Report',
                            reportDir: 'dist',
                            reportFiles: 'index.html',
                            keepAll: true,
                            alwaysLinkToLastBuild: true
                        ])
                    } else {
                        echo "dist/index.html not found, skipping HTML publish"
                    }
                }
            }
        }
    }

    post {
        success {
            emailext(
                to: "${EMAIL_RECIPIENT}",
                subject: "Jenkins Build Successful: ${currentBuild.fullDisplayName}",
                body: "Good news! The build succeeded.\nCheck console output: ${env.BUILD_URL}"
            )
        }
        failure {
            emailext(
                to: "${EMAIL_RECIPIENT}",
                subject: "Jenkins Build Failed: ${currentBuild.fullDisplayName}",
                body: "Oops! The build failed.\nCheck console output: ${env.BUILD_URL}"
            )
        }
    }
}
