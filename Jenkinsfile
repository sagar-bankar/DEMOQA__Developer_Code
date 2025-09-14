pipeline {
    agent any

    tools {
        maven 'MyMaven'
        jdk 'MyJava'
    }

    stages {
        stage('Checkout Developer Code') {
            steps {
                git branch: 'master', url: 'https://github.com/sagar-bankar/DEMOQA_Developer_Code.git'
            }
        }

        stage('Checkout Tester Code') {
            steps {
                dir('tester-repo') {
                    git branch: 'master', url: 'https://github.com/sagar-bankar/DEMOQA__Automation-Engineer.git'
                }
            }
        }

        stage('Build') {
            steps {
                dir('tester-repo') {
                    bat 'mvn clean install -DskipTests'
                }
            }
        }

        stage('Run Tests') {
            steps {
                dir('tester-repo') {
                    bat 'mvn test'
                }
            }
        }

        stage('Archive Reports') {
            steps {
                dir('tester-repo') {
                    junit 'target/surefire-reports/*.xml'
                    archiveArtifacts artifacts: 'reports/*.html', fingerprint: true
                }
            }
        }
    }
}
