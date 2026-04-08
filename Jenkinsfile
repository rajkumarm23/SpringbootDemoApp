pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from SCM
                checkout scm
            }
        }

        stage('Build') {
            steps {
                // Compile the code
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                // Run tests
                sh 'mvn test'
            }
        }

        stage('Package') {
            steps {
                // Package the application
                sh 'mvn package'
            }
        }

        stage('Deploy') {
            steps {
                // Run the Spring Boot application
                sh 'nohup java -jar target/DempApplication-0.0.1-SNAPSHOT.jar > app.log 2>&1 &'
                sh 'sleep 5'
                sh 'echo "Application started. Check app.log for logs"'
            }
        }
    }

    post {
        always {
            // Archive test results
            junit 'target/surefire-reports/*.xml'
        }
        success {
            // Archive the jar
            archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
        }
    }
}
