pipeline {
    agent any

    environment {
        // ✅ Tomcat Configuration
        TOMCAT_PATH = 'C:\\Program Files\\Apache Software Foundation\\Tomcat 9.0\\webapps'
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
                    bat '''
                    echo Deleting old WAR...
                    del "%TOMCAT_PATH%\\hospital.war" || echo No previous WAR found

                    echo Renaming and copying new WAR...
                    ren hospital-1.0.0.war hospital.war

                    echo Copying WAR to Tomcat folder...
                    copy "hospital.war" "%TOMCAT_PATH%" /Y

                    echo ✅ WAR Deployed Successfully!
                    '''
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
            echo '🎉 Deployment completed successfully! Tomcat should auto-deploy the WAR!'
        }
        failure {
            echo '❌ Deployment failed. Check logs or WAR copy step!'
        }
    }
}
