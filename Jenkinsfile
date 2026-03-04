pipeline {
    agent any

    tools {
        jdk 'Java8'
        maven 'Maven3'
    }

    environment {
        MULE_HOME = "/Users/alphanove/Downloads/Softwares/mule-standalone-4.5.0"
    }

    stages {

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

        stage('Stop Mule') {
            steps {
                echo "Stopping Mule runtime..."
                sh '''
                if pgrep -f mule >/dev/null; then
                    $MULE_HOME/bin/mule stop
                    sleep 10
                else
                    echo "Mule not running"
                fi
                '''
            }
        }

        stage('Deploy New App') {
            steps {
                echo "Deploying new version..."
                sh '''
                APP_JAR=$(ls target/*.jar | head -n 1)
                echo "Deploying $APP_JAR"
                cp $APP_JAR $MULE_HOME/apps/
                '''
            }
        }

        stage('Start Mule') {
            steps {
                echo "Starting Mule runtime..."
                sh '''
                $MULE_HOME/bin/mule start
                sleep 60
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                echo "Checking if app is running..."
                sh '''
                STATUS=$(curl -k -s -o /dev/null -w "%{http_code}" https://localhost:8081/test)
                if [ "$STATUS" = "200" ]; then
                    echo "Deployment SUCCESSFUL"
                else
                    echo "Deployment FAILED"
                    exit 1
                fi
                '''
            }
        }
    }
}