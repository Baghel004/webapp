pipeline {
    agent any

    tools {
        maven 'Maven'   // Must match name in Global Tool Configuration
    }

    environment {
        SONAR_HOST_URL = 'http://localhost:9000'
    }

    stages {

        stage('Clean & Build') {
            steps {
                bat 'mvn clean install'
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
    }

    post {
        success {
            echo 'BUILD SUCCESS 🚀'
        }
        failure {
            echo 'BUILD FAILED ❌'
        }
    }
}
