pipeline {
    agent any

    tools {
        jdk 'Java8'
        maven 'Maven3'
    }

    environment {
        APP_NAME = "Mule4_HTTPS_Listener_POC"
        MULE_HOME = "/Users/alphanove/Downloads/Softwares/mule-standalone-4.5.0"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Mule App') {
            steps {
                echo "Building Mule application using Java 8..."
                sh 'java -version'
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Verify Artifact') {
            steps {
                echo "Listing generated artifacts..."
                sh 'ls -l target/'
            }
        }

        stage('Deploy to Mule Runtime') {
            steps {
                echo "Deploying to Mule runtime..."
                sh """
                cp target/*-mule-application.jar ${MULE_HOME}/apps/
                """
            }
        }

    }

    post {
        success {
            echo "Build & Deployment Successful ✅"
        }
        failure {
            echo "Build Failed ❌"
        }
    }
}