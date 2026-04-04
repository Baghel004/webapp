pipeline {
    agent any

    tools {
        maven 'Maven'
    }

    environment {
        SONAR_HOST_URL = 'http://localhost:9000'
    }

    stages {

        stage('Clean & Build') {
            steps {
                bat 'mvn clean install -DskipTests'
            }
        }

        // ✅ TEST STAGE (YOU MISSED THIS)
        stage('Test') {
            steps {
                bat 'mvn test'
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
                    bat """
                    mvn sonar:sonar ^
                    -Dsonar.host.url=%SONAR_HOST_URL% ^
                    -Dsonar.login=%SONAR_TOKEN%
                    """
                }
            }
        }

        // ✅ DEPLOY STAGE (ASSIGNMENT PART)
        stage('Deploy') {
            steps {
                bat 'C:\\deployment\\deployment.bat'
            }
        }
    }

    post {
        success {
            echo 'BUILD + TEST + DEPLOY SUCCESS 🚀'
        }
        failure {
            echo 'PIPELINE FAILED ❌'
        }
    }
}
