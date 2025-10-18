pipeline {
    agent any

    environment {
        // Change EMAIL_RECIPIENT to raimund's email or your own
        EMAIL_RECIPIENT = 'raimund@example.com'
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

        stage('Publish HTML Reports') {
            steps {
                publishHTML(target: [
                    reportName: 'App Report',
                    reportDir: 'dist',
                    reportFiles: 'index.html',
                    keepAll: true,
                    alwaysLinkToLastBuild: true
                ])
            }
        }
    }

    post {
        success {
            mail to: "${EMAIL_RECIPIENT}",
                 subject: "Jenkins Build Successful: ${currentBuild.fullDisplayName}",
                 body: "Good news! The build succeeded.\nCheck Jenkins console output here: ${env.BUILD_URL}"
        }
        failure {
            mail to: "${EMAIL_RECIPIENT}",
                 subject: "Jenkins Build Failed: ${currentBuild.fullDisplayName}",
                 body: "Oops! The build failed.\nCheck Jenkins console output here: ${env.BUILD_URL}"
        }
    }
}
