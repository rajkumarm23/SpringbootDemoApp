pipeline {
    agent any

    tools {
        maven 'Maven3'
        jdk 'JDK17'
    }

    environment {
        TOMCAT_WEBAPPS = "C:\\Program Files\\Apache Software Foundation\\Tomcat 11.0\\webapps"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git 'https://github.com/rajkumarm23/SpringbootDemoApp.git'
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean package'
            }
        }

        stage('Test Report') {
            steps {
                junit 'target/surefire-reports/*.xml'
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'target/*.war', fingerprint: true
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                bat """
                copy target\\Demo_App.war %TOMCAT_WEBAPPS%
                """
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful!'
        }
        failure {
            echo 'Build Failed!'
        }
    }
}
