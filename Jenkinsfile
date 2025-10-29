pipeline {
    agent any

    environment {
        // Adjust Tomcat details below 👇
        TOMCAT_USER = 'admin'
        TOMCAT_PASS = 'admin'
        TOMCAT_HOST = 'http://localhost:8080'
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
                echo '🚀 Deploying WAR to Tomcat server...'
                dir('hospital-backend-jenkins-main/target') {
                    // delete old deployment
                    bat """
                    curl -u %TOMCAT_USER%:%TOMCAT_PASS% "%TOMCAT_HOST%/manager/text/undeploy?path=/hospital" || echo 'No previous deployment'
                    curl -u %TOMCAT_USER%:%TOMCAT_PASS% -T %WAR_NAME% "%TOMCAT_HOST%/manager/text/deploy?path=/hospital&update=true"
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
