pipeline {
    agent any

    environment {
        // ✅ Tomcat details (adjust path if needed)
        TOMCAT_PATH = 'C:\\Program Files\\Apache Software Foundation\\Tomcat9\\webapps'
        WAR_NAME = 'hospital.war'
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
                echo '🚀 Deploying WAR to Tomcat (Windows)...'
                dir('hospital-backend-jenkins-main\\target') {
                    bat """
                    echo Removing old deployment...
                    del "%TOMCAT_PATH%\\hospital.war"
                    rmdir /s /q "%TOMCAT_PATH%\\hospital"
                    
                    echo Copying new WAR file...
                    copy "%WAR_NAME%" "%TOMCAT_PATH%"
                    """
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                echo '🔍 Verifying deployment...'
                bat 'curl -I http://localhost:8080/hospital || echo "⚠️ Application not reachable yet!"'
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
