
pipeline {
    agent any

    environment {
        JAVA_HOME = 'C:\\Program Files\\Java\\jdk-25'
        PATH = "${JAVA_HOME}\\bin;${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Check Environment') {
            steps {
                bat 'java -version'
                bat 'javac -version'
                bat 'mvn -version'
            }
        }

        stage('Build and Test') {
            steps {
                bat 'mvn -B clean test'
            }
            post {
                always {
                    junit allowEmptyResults: true,
                          testResults: 'course-registration-app/target/surefire-reports/TEST-*.xml'
                }
            }
        }

        stage('Package') {
            steps {
                bat 'mvn -B package -DskipTests'
            }
            post {
                success {
                    archiveArtifacts(
                        artifacts: 'course-registration-app/target/*.jar',
                        fingerprint: true
                    )
                }
            }
        }
    }
}
