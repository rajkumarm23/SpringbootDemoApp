pipeline {
    agent { label 'windows' }

    tools {
        maven 'Maven'
        jdk 'JDK'
    }

    environment {
        TOMCAT_WEBAPPS = "C:\\Program Files\\Apache Software Foundation\\Tomcat 11.0\\webapps"
        JAVA_HOME = '/usr/lib/jvm/java-17-openjdk-amd64'
        PATH = "/usr/lib/jvm/java-17-openjdk-amd64/bin:${env.PATH}"
    }

    stages {

        stage('Check Java') {
                    steps {
                        sh 'echo JAVA_HOME=$JAVA_HOME'
                        sh 'java -version'
                    }
                }


        stage('Checkout Code') {
            steps {
                git 'https://github.com/rajkumarm23/SpringbootDemoApp.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
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
