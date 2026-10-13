
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build and Test') {
            steps {
                bat 'mvn -B clean test'
            }
            post {
                always {
                    script {
                        def reports = findFiles(
                            glob: 'course-registration-app/target/surefire-reports/TEST-*.xml'
                        )

                        if (reports.length > 0) {
                            junit 'course-registration-app/target/surefire-reports/TEST-*.xml'
                        } else {
                            echo 'No JUnit XML reports found. Check the Maven test output and workspace.'
                        }
                    }
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
