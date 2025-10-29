pipeline {
    agent any

    environment {
        // Tomcat details 👇
        TOMCAT_USER = 'admin'
        TOMCAT_PASS = 'admin'
        TOMCAT_HOST = 'http://localhost:8080'
        WAR_NAME = 'hospital.war'
        TOMCAT_PATH = 'C:\\Program Files\\Apache Software Foundation\\Tomcat 9.0\\webapps'
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo '✅ Cloning repository...'
                git branch: 'main', url: 'https://github.com/2300031388/CICD_Exam.git'
            }
        }

        stage('Build Backend') {
            steps {
                dir('hospital-backend-jenkins-main') {
                    echo '⚙️ Building backend using Maven...'
                    bat 'mvn clean package -DskipTests'
                }
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                echo '🚀 Deploying WAR to Tomcat server...'
                dir('hospital-backend-jenkins-main/target') {
                    bat """
                    echo Deleting old WAR...
                    del "%TOMCAT_PATH%\\%WAR_NAME%" || echo No previous WAR found
                    echo Copying new WAR...
                    copy "%WAR_NAME%" "%TOMCAT_PATH%\\" /Y
                    """
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                echo '🔍 Checking deployment status...'
                bat 'curl -I http://localhost:8080/hospital || echo "Check failed"'
            }
        }
    }

    post {
        success {
            echo '🎉 Deployment completed successfully!'
        }
        failure {
            echo '❌ Deployment failed. Check logs!'
        }
    }
}
