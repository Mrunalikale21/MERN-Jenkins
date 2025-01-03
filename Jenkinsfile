pipeline {
    agent any
    tools {
        nodejs "Node" // Ensure "Node" is the correct name configured in Jenkins
    }
    environment {
        SONAR_TOKEN = credentials('sonar-token') // Ensure 'sonar-token' credential exists in Jenkins
    }
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Mrunalikale21/MERN-Jenkins.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }
        stage('Install pm2') {
            steps {
                bat 'npm install pm2 -g'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonarqube') { // Ensure "sonarqube" matches the name configured in Jenkins
                    bat """
                    sonar-scanner \
                      -Dsonar.projectKey=mern-jenkins \
                      -Dsonar.sources=src \
                      -Dsonar.host.url=http://localhost:9000 \
                      -Dsonar.login=${SONAR_TOKEN}
                    """
                }
            }
        }
    }
    post {
        success {
            echo 'Pipeline completed successfully'
        }
        failure {
            echo 'Pipeline failed'
        }
        always {
            echo 'This runs regardless of the result.'
        }
    }
}
