pipeline {
    agent any

    tools {
        maven 'Maven'   // Make sure this is configured in Jenkins
    }

    environment {
        APP_NAME = 'DempApplication'
        JAR_FILE = 'target/DempApplication-0.0.1-SNAPSHOT.jar'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build & Test') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Verify JAR') {
            steps {
                sh '''
                echo "Checking target folder..."
                ls -l target/

                if [ ! -f "$JAR_FILE" ]; then
                    echo "JAR file not found!"
                    exit 1
                fi

                echo "JAR file exists: $JAR_FILE"
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                echo "Stopping old application if running..."
                pkill -f $APP_NAME || true

                echo "Starting new application..."
                setsid java -jar $JAR_FILE > app.log 2>&1 < /dev/null &

                sleep 10

                echo "===== Application Logs ====="
                if [ -f app.log ]; then
                    cat app.log
                else
                    echo "app.log not found — application may have failed to start"
                fi
                '''
            }
        }

        stage('Health Check') {
            steps {
                sh '''
                echo "Checking application via HTTP..."

                sleep 5

                if curl -s http://localhost:8888 > /dev/null; then
                    echo "Application is running successfully"
                else
                    echo "Application is NOT reachable"
                    exit 1
                fi
                '''
            }
        }

    }

    post {
        always {
            echo "Archiving test results..."
            junit 'target/surefire-reports/*.xml'
        }

        success {
            echo "Archiving JAR..."
            archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
        }

        failure {
            echo "Build failed. Check logs above."
        }
    }
}
