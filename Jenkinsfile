pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Pulling Java project from GitHub...'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Compiling Java source code...'
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                echo 'Running JUnit tests...'
                sh 'mvn test'
            }
            post {
                always {
                    // Publishes JUnit test results directly in the Jenkins UI
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }

        stage('Package') {
            steps {
                echo 'Packaging application into a JAR file...'
                sh 'mvn package -DskipTests'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }

    post {
        success {
            echo 'Java Maven Pipeline succeeded!'
        }
        failure {
            echo 'Build or tests failed. Check Console Output.'
        }
    }
}
