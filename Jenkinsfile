pipeline {
    agent any

    tools {
        maven 'MyMaven'
        jdk 'MyJava'
    }

    stages {
        stage('Trigger Info') {
            steps {
                echo "✅ Pipeline triggered by Developer repo webhook 🚀"
            }
        }

        stage('Checkout Tester Code') {
            steps {
                git branch: 'master', url: 'https://github.com/sagar-bankar/DEMOQA__Automation-Engineer.git'
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean install -DskipTests'
            }
        }

        stage('Run Tests') {
            steps {
                bat 'mvn test'
            }
        }

        stage('Archive Reports') {
            steps {
                junit 'target/surefire-reports/*.xml'
                archiveArtifacts artifacts: 'reports/*.html', fingerprint: true
            }
        }
    }

    post {
        always {
            echo 'Cleaning workspace...'
            cleanWs()
        }
        failure {
            echo 'Build failed. Sending email...'
            emailext (
                to: 'sagar.bankar590@gmail.com',
                subject: "❌ Build Failed - ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
                    Hello Team,<br><br>
                    Build/Tests FAILED ❌<br><br>

                    <b>Test Summary:</b><br>
                    - Total Tests: \${TEST_COUNTS,total}<br>
                    - Passed: \${TEST_COUNTS,passed}<br>
                    - Failed: \${TEST_COUNTS,failed}<br>
                    - Skipped: \${TEST_COUNTS,skipped}<br><br>

                    Please check Jenkins console logs.<br><br>
                    <b>Job:</b> ${env.JOB_NAME}<br>
                    <b>Build Number:</b> #${env.BUILD_NUMBER}<br>
                    <b>Build URL:</b> <a href="${env.BUILD_URL}">${env.BUILD_URL}</a>
                """,
                attachmentsPattern: 'reports/*.html'
            )
        }
        success {
            echo 'Build successful! Sending email...'
            emailext (
                to: 'sagar.bankar590@gmail.com',
                subject: "✅ Build Success - ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
                    Hello Team,<br><br>
                    All tests executed successfully ✅<br><br>

                    <b>Test Summary:</b><br>
                    - Total Tests: \${TEST_COUNTS,total}<br>
                    - Passed: \${TEST_COUNTS,passed}<br>
                    - Failed: \${TEST_COUNTS,failed}<br>
                    - Skipped: \${TEST_COUNTS,skipped}<br><br>

                    Please find the attached test report.<br><br>
                    <b>Job:</b> ${env.JOB_NAME}<br>
                    <b>Build Number:</b> #${env.BUILD_NUMBER}<br>
                    <b>Build URL:</b> <a href="${env.BUILD_URL}">${env.BUILD_URL}</a>
                """,
                attachmentsPattern: 'reports/*.html'
            )
        }
    }
}
