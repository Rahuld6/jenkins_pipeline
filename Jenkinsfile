pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                echo 'Repository cloned successfully!'
            }
        }

        stage('Build Project') {
            steps {
                echo 'Building the project...'
                sh 'echo "mvn clean package would run here"'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                echo 'Deploying to Tomcat server...'
                sh 'echo "WAR file deployed to Tomcat"'
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed!'
        }
    }
}

