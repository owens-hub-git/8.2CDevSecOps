pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
            git branch: 'main', url: ' https://github.com/owens-hub-git/8.2CDevSecOps.git'
            }
        }
        stage('Install Dependencies') {
            steps {
            sh 'npm install'
            }
        }
        stage('Run Tests') {
            steps {
                sh 'npm test || true'
            }
            post {
                always {
                    emailext (
                        to: 'costio2004@gmail.com',
                        subject: "Stage: Run Tests — ${currentBuild.currentResult}",
                        body: """Run Tests finished with: ${currentBuild.currentResult}
                        Full console log:
                        ${env.BUILD_URL}console 
                        Build URL: 
                        ${env.BUILD_URL}""",
                        attachLog: true // Attach the build log
                    )
                }
            }
        }
        stage('Generate Coverage Report') {
            steps {
            // Ensure coverage report exists
            sh 'npm run coverage || true'
            }
        }
        stage('NPM Audit (Security Scan)') {
            steps {
                sh 'npm audit || true'
            }
            post {
                always {
                    emailext (
                        to: 'costio2004@gmail.com',
                        subject: "Stage: Security Scan — ${currentBuild.currentResult}",
                        body: """Security Scan finished with: ${currentBuild.currentResult}
                        Full console log:
                        ${env.BUILD_URL}console 
                        Build URL: 
                        ${env.BUILD_URL}""",
                        attachLog: true // Attach the build log
                    )
                }
            }
        }
    }
}